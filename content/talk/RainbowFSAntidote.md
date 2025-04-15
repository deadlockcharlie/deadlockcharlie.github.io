+++
title= "Designing and Implementing a Persistent Cache for a CRDT Data store"
venue= "Sorbonne University"
date= 2022-03-28
location= "Paris, France"
+++

This talk was given as a part of the workshop for [RainbowFS](https://rainbowfs.gitlabpages.inria.fr/final-workshop/). 

## Abstract

Most in-memory databases offer object-persistence through transaction logging and snapshots. During routine restarts, the memory snapshots are read from the disk and stored in the memory for accesses. When failures happen, the logs help in recovery to undo or redo transactions.

There is another way of managing objects in databases. Instead of writing snapshots to memory and using logs as a fallback mechanism, we can use logs as the primary source of objects’ states and use snapshots to fallback on in case of failures. This strategy is implemented in Journal based databases.

In our work, we present the design of a cache and a persistent checkpoint store for a database which uses the log to create objects’ states. We study how to materialize the objects from the journal using other snapshots while maintaining causal dependencies and keeping these snapshots in memory to improve transaction performance. The design is supported by benchmarks comparing the design with a reference implementation. 

[slides PDF](/talk/slides/RainbowFS.pdf)