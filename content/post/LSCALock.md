+++
title= 'Dominance Locking in arbitrary rooted hierarchies'
date= 2022-06-16
tags = ["Locking", "Concurrency", "Hierarchies"]
description = "A short study of hierarchical locking"
author = "Ayush Pandey"
+++
<!--more-->
The current state of affairs in databases is typically oriented towards two-dimensional data. If some data is managed in an apparent multidimensional space, then it is usually achieved by some trick that involves joining tables and pivoting them. This not only makes performing queries on the data difficult and less efficient but also introduces an overhead of managing these multidimensional views.

To add to the problem, we have concurrent systems that use this data and this require synchronization. One way to ensure a safe state with concurrency is to ensure that the operations on the data itself are commutative and can thus be compensated on failures. But not only is commutativity tricky to achieve but it also needs to be implemented with specific modifications to the underlying data structures for example, by using CRDTs.

Another way that threads use to synchronize is by using locks. Locks have been used in practice on large scale databases with proven success. They are robust but what happens when we try to lock hierarchies? A hierarchy is dimensionally spread out and can allow queries and modifications from different dimensions. Given this situation, when a thread wished to lock a node in a hierarchy, it needs a lock of a configurable granularity. This is commonly referred to as __Multi Granularity Locking__. 


Multi Granularity Locking (MGL)
======
MGL is a technique with which the position of the lock in a hierarchy determines its impact on the hierarchy. So a lock placed on a leaf node locks only the leaf node while a lock placed on the root locks the entire hierarchy. MGL locks the entire hierarchy or a subset of it to prevent fine locks and allows concurrent access to non overlapping parts of the hierarchy. The problem with MGL is that the traversal cost to find overlaps and lock structures is high.

Intention Locking (IL)
------

When locking hierarchies, [Intention locking](https://doi.org/10.1145/1282480.1282513) is a commonly used MGL technique. A thread that wishes to lock a specific node starts traversing the hierarchy from the root and places an _intention lock_ on all the nodes it encounters on the path to the target node it wishes to lock. Once the target node is locked with the appropriate lock type, the intention locks on the path can be released so that the other threads can progress. 

Evidently IL is not efficient for big hierarchies because it involves traversing the hierarchy and locking all the nodes on the path to the target node. This is equivalent to performing an all path DFS for the target node. Also, IL locks the nodes on the path which restricts the concurrency. Dynamic modifications to the hierarchy can also break intention locking because a paths used for IL can be changed by another thread adding an edge.

Problem
======

To lock hierarchies an ideal technique should have the following properties:

- The lock acquisition cost should be minimum i.e., the number of locks acquired should be as low as possible.
- The placement of the lock on the hierarchy should not take orders of magnitude longer than the number of nodes required to lock. 


There are a few properties of hierarchies that we can use to our advantage to achieve the above goals. As mentioned in the [DomLock](/posts/2022/05/domlock/) summary, a hierarchy has a ***containment*** relationship between objects where one object is said to contained in another or in other words, a child. We can use the containment property to lock nodes just like [DomLock](/posts/2022/05/domlock/) does. But DomLock is not robust to modifications in the hierarchy. It involves BFS assignment of intervals which limits intermediate modifications to the hierarchy. We still use the concept of dominators but modify it to use path decompositions instead of intervals. 


Dominators
------
For any graph, a node ***d*** is a **Dominator** of node ***n*** if all the paths from the ***root*** to node ***n*** pass through ***d***.

A dominator ***d*** is an **Immediate Dominator** of node ***n*** if there exists no other dominator on the paths between ***d*** and ***n***.

This gives us a few trivial relationships in specific data structures. 
- For trees, each node's parent is its immediate dominator. 
- In trees, for a set of nodes, their lowest common ancestor is the dominator. 
- In a DAG, nodes that belong to all the reference paths become the dominators.
- When there are cycles, there can be infinite dominators. In such a case, the node of entry to the cycle is considered the dominator of the nodes in the cycle. 
