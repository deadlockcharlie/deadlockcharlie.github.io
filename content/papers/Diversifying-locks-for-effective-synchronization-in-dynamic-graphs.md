---
title: "Diversifying locks for effective synchronization in dynamic graphs"
date: 2024-04-01
venue: 'EuroSys 2024 Doctoral Workshop'
paperurl: 'https://inria.hal.science/hal-04851918'
citation: ' Ayush Pandey,  Julien Sopena,  Swan Dubois,  Marc Shapiro, &quot;Diversifying locks for effective synchronization in dynamic graphs.&quot; EuroSys 2024 Doctoral Workshop, 2024.'
---
Summary
------
A graph database contains complex data, which application transactions access concurrently. Maintaining correctness under concurrent updates requires some form of synchronization; locking is widely used for this purpose. However, the locking techniques used in graph databases suffer from some drawbacks. Locking techniques that utilize metadata to exploit the topology of the graph for locking have limited scalability over dynamic graphs owing to the cost of maintaining the metadata. Most locking techniques also use the classical read/write locks, which are not sufficient to facilitate maximum concurrency due to the complex nature of trivial graph operations. Our work aims to address these issues by proposing a new extensible labeling scheme called CALock for large, rooted graphs that allows efficient lock grain identification in dynamic graphs. We also aim to introduce new, richer and finer lock types for graph operations that allow for more concurrency while maintaining the same synchronization level. In this paper, we present the current state of our work, the preliminary results and the future work we intend to do. 

[Access paper here](https://inria.hal.science/hal-04851918)
