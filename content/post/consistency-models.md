+++
title =  "Consistency models and their interplay"
date = 2025-04-04
description = "A short article comparing the different consistency models as defined in the literature"
tags = ["distributed systems", "consistency models", "databases"]
author = "Ayush Pandey"
+++
<!--more-->

Consistency models are difficult to explain and even more difficult to understand. At the time of writing, there is about 20 consistency levels that I know of, each with its own set of assumptions and implications which in a lot of cases are only subtly different between different consistency levels. Adding to this complexity is the fact that different communities refer to different things with the same name. Take Snapshot Isolation(SI) for example. To a relational database practitioner, SI means concurrent snapshots that remain isolated from each other. It is not really a consistency level but rather the Isolation property in the ACID mode. But for a distributed systems practitioner, SI means something completely different.
In an attempt to make these definitions clear for myself and hopefully for you, dear reader; I am writing this post.

The content here is mostly based on the definitions from the following sources. 
1. [Consistency in Non-Transactional Distributed Storage Systems](https://arxiv.org/pdf/1512.00168)
2. [Highly Available Transactions: Virtues and Limitations](https://www.vldb.org/pvldb/vol7/p181-bailis.pdf)
3. [Consistency Models](https://jepsen.io/consistency/models)
  


![Consistency models](/images/consistency-models.svg)
{.centered-img}
 <!-- {{< figure src="/images/consistency-models.svg" title="Consistency models, weakest to strongest (source: Jepsen.io)" width="40%" align="center" style="text-align: center, filter:invert(1)">}}  -->


Fundamentally, a consistency level is a set of predicates, evaluated over a history. The evaluation result of the predicates dictates if the history adheres to that consistency level i.e. that history is "legal" or "valid".

The more restrictive the consistency level (towards the top in the hierarchy of consistency levels), the stronger it is.

### Weak and eventual consistency [[Viotti et. al. ](https://arxiv.org/pdf/1512.00168)]

Traditionally, weak consistency refers to any consistency level which is weaker than sequential consistency. However, recent developments in the consistency level definitions have associated it to a very specific definition.  A weakly consistency system does not guarantee that reads return the most recent value written and several requirements have to be satisfied for a value to be returned. Weak consistency levels do not provide ordering guarantees do often, a synchronization protocol is not required. While seemingly useless, weak consistency is often used in relaxed caching policies that can be applied to various tiers of a web application. 

Eventual consistency is slightly stronger than weak consistency. Under EC, replicas converge towards identical copies in the absence of further updates. If no new writes occur, eventually all writes will return the same value. 

[Shapiro et. al.](http://dx.doi.org/10.1007/978-3-642-24550-3_29) identify an alternate definition of EC from a replica's POV. 

* Eventual delivery: if some correct replica applies a write operation _op_, _op_ is eventually applied by all correct replicas.
* Convergence: all correct replicas that have applied the same write operations eventually reach equivalent state.
* Termination: all operations complete.


To this definition, if we add Strong convergence i.e. all correct replicas that have applies the same write operations have equivalent state, we get **Strong eventual consistency**.


### Read uncommitted
As the name suggests, it allows a transaction to read uncommitted updates from other transactions. Ergo, it can read concurrent uncommitted updates, perform a computation based on those updates. Meanwhile, the transaction whose update was observed might fail, causing an inconsistent data state. 

This is probably the weakest consistency level which is guaranteed by all databases. 

There is one caveat though, this definition is according to the ANSI SQL 1999 spec. In the distributed systems community, we prefer the definition laid down by Adya in his [thesis](https://pmg.csail.mit.edu/papers/adya-phd.pdf), which prevents dirty writes. 

### Read committed

It prevents dirty reads. Only committed updated become visible. RC is totally available. Even the presence of network partitions, the system can make progress. 

### Monotonic atomic view

It strengthens RC by preventing transactions from observing some but not all of a previously committed transaction's effects. It is essentially the Atomicity constraint of the ACID model. Sometimes also referred to as Atomic visibility. 

Monotonic atomic view is totally available. 

### Cursor stability

It strengthens the read committed consistency model by preventing lost updates. 

It is a transactional model and introduces pointers called 'cursors' which a transaction uses to refer to an object it is reading. When a transaction reads an object using a cursor, that object cannot be modified until the cursor is released or the transaction commits. 

### Repeatable reads

It restricts the value returned by a read within a transaction. If a transaction returns the value v for a read r(x) then any subsequent r(x) must return v unless the transaction itself modifies x. Any concurrent modification to x by another transaction will not be visible to this transaction after it has read the value of x.  

Repeatable reads is unavailable when network partitions occur. 

### Snapshot isolation

In SI, each transaction appears to operate on an independent, consistent snapshot of the database. Its changes are visible only to itself until it commits. After commit, any transaction reading from this commit will observe the changes the transaction made. 

SI can be implemented by either aborting one of many concurrent transactions that have overlapping write sets or by using merge semantics which arbitrate concurrent writes.

SI is not available when network partitions occur. 

### Serializability

Dictates that transactions appear to have occurred in some total order.  The operations within a transaction appear to occur atomically.

Serializability implies Repeatable reads and snapshot isolation. Serializability is not available when network partitions occur. 




### Monotonic reads

It ensures that if a process performs r1 then r2, then r2 cannot observe a state prior to the writes which were reflected in r1, i.e. reads cannot go backwards. Monotonic reads is a per process/transaction restriction. 

### Monotonic writes

It ensures that if a process performs write w1, then w2, then all processes observe w1 before w2. Like monotonic reads, monotonic writes is a per process/transaction restriction. 

### Writes follow reads

It is sometimes also called as session causality. This model restricts the order in which writes become visible. If a process reads a value v which comes from a write w1, and later performs a write w2, then w2 must become visible after w1. Once you read something, you can't change its past.


### Read your writes

It requires that if a process performs a write w, then any subsequent read must observe w's effects until another write is performed which then must be visible. 

RYW is sticky available. Everything before, is totally available. In RYW, a system remains available so long as the clients never change the server they talk to. 


### PRAM (Pipeline Random Access Memory)

It attempts to relax existing coherent memory models to obtain better concurrency. It enforces that any pair of writes executed by a single process/transaction are observed (everywhere) in the order the process executed them; However, writes from different processes may be observed in different orders. 

PRAM is a combination of Monotonic reads, Monotonic writes and Read you writes. It is sticky available. If clients do not change the server they talk to, the system can make progress. 


### Causal consistency

It is a notion that causally related operations should appear in the same order on all processes/transactions-though processes may disagree about the order of causally unrelated operations. 

It stems from Lamport's happens-before relation which captures the notion of potential causality by relating an operation to previous operations by the same process (Session order) and to the operations of other process whose effects could have been visible (write-read relation or messages).

Causal consistency is sticky available. Even in the presence of network partitions, any non-faulty node can make progress if clients stick to the same server. It is the strongest consistency level which remains available  

A stronger variant of causal consistency is [**Real-time causal**](https://www.cs.cornell.edu/lorenzo/papers/cac-tr.pdf) which, in addition to the restriction on arbitration of causally related operations, also restricts the real time ordering of operations. CC3 condition states that time does not go backwards. So if the end time of an operation u is earlier than the begin time of an operation v, then v cannot happen-before u, even though they might be causally unrelated. 

Causal consistency is a combination of PRAM and Writes follow reads, i.e. CC = Monotonic reads + Monotonic writes + Read your writes + Writes follow reads. 

### Sequential consistency

It is a strong consistency level. Informally, it dictates that all operations take effect in the some total order which is consistent with the order of operations on each process/transaction. 

Sequential consistency is unavailable when network partitions occur. 

A process in a sequentially consistent system may be far ahead or far behind of other processes. However, once a process observes some operation from another process, it cannot observe a stale state. Sequential consistency is a particularly strong model for programmers. 



### Linearizability and related "strong" consistency levels [[Viotti et. al](https://arxiv.org/pdf/1512.00168)]

Linearizability dictates that each operation shall appear to take effect instantaneously at a point between its invocation and its response. Linearizability has long been considered the gold standard of consistency for a system. Interestingly, a composition of linearizable objects is also linearizable. However, in practice, linearizability is very difficult to implement without sacrificing availability. 

Another closely related definition was proposed by Lamport for the atomic register semantics. Lamport describes a single-writer multi-reader shared register to be atomic iff each read operation, not overlapping a write returns the last value actually written to the register. 

Two slightly "weaker" variants of SWMR registers are: **safe** and **regular**. In absence of read-write concurrency, both follow the semantics of an atomic register. The difference between the three is the allowed set of return values in the presence of read-write concurrency. 

- A safe register may return any value.
- A read, concurrent with a write can return either the value written by the most recent complete write, or a value written by a concurrent write. 

### Strict Serializability

Strong Serializability or Strict Serializability means that operations appear to have occured in some order consistent with the real-time order of operations and should appear in the same order across all processes. 

SS implies serializability and linearizability. It is not available when network partitions occur




Common Confusions
==================

### Repeatable reads vs Snapshot isolation

Repeatable reads is a restriction on the read set of a transaction. When reading, a transaction is free to arbitrarily choose a dependency to read from. Once chosen, a subsequent read should read from this dependency. 

SI on the other hand is a restriction on the write set of a transaction. Two concurrent transactions with overlapping write sets cannot commit. One of them has to abort. 


### Serializability and Linearizability

Serializability dictates that transactions appear to have occurred in some total order. Since a transaction becomes visible at commit, the linearization point of this transaction is at commit. In-transaction operations become visible in session order and intra-transaction operations according to the commits. 

Linearizability is non transactional. Since Linearizability is a single-object model, a transaction's writes to an object can be linearized before its commit. As such it may become visible before commit. In addition, Linearizability dictates that the linearization order of operations is consistent with their real-time order.
