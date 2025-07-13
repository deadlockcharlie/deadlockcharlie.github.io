+++
title= "Verrouillage multi-granularité dans les graphes orientés"
venue= "ComPAS 2023"
date= 2023-07-05
location= "Annecy, France"
+++

This talk was given as a presentation in [ComPAS](https://2023.compas-conference.fr/). 

## Abstract

With the increasing demand for parallel processing and shared data structures in modern computer systems, the need for effective synchronization mechanisms has become increasingly important. Concurrent accesses to shared data require synchronization, and locking is a widely used strategy in this context. Multi-granularity locking approaches have been proposed to allow threads to lock the whole sub-graph concerning a query in a single lock acquisition, particularly when the data structure is hierarchical. However, existing strategies often suffer from performance limitations.

To address this challenge, this talk presents CALock, a new labelling strategy for multi-granularity locking that exploits the topology of a rooted directed graph to identify the finest lock grain. The CALock approach is evaluated experimentally, and the results demonstrate that it offers higher concurrency and throughput than similar approaches when the underlying graph is dynamic. In particular, CALock exhibits a 200\% increase in throughput compared to similar locking approaches for workloads with contended access requests.


[slides PDF](/talk/slides/CALock_Compas.pdf)