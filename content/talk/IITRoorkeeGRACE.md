+++
title= "Property graph replication with invariant preservation"
venue= "IIT Roorkee"
date= 2025-11-12
location= "Roorkee, India"
+++


This talk was given as a part of a seminar. 

## Abstract

Graph databases are widely used in domains such as fraud detection, recommendation systems, social networks, and life sciences. As applications become globally distributed, geo-replicating graph data is necessary for availability and low latency. However, strong consistency requires costly coordination, while weak consistency risks breaking invariants. Our work introduces the Property Graph CRDT (PG-CRDT), a coordination-free replicated data type for property graphs that ensures strong eventual consistency and rollback-freedom. This work implements PG-CRDT in a middleware that is agnostic to the underlying graph database. Experiments on synthetic and real-world workloads show that our approach preserves invariants, remains available under partitions, and achieves upto 2 orders-of-magnitude lower write latency while keeping read latency unaffected.