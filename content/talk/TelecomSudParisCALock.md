+++
title= "CALock: Topological multi-granularity locking in hierarchical data"
venue= "Parallel and Distributed Systems Group Telecom SudParis"
date= 2024-12-06
location= "Paris, France"
+++


This talk was given as a part of a seminar (https://www.inf.telecom-sudparis.eu/pds/seminars_cpt/983/). 

## Abstract

Hierarchies serve as fundamental structures across various disciplines, modeling hierarchical relationships in computer science, biology, social networks, and logistics. However, the dynamic and concurrent updates in real-world systems necessitate synchronization techniques for maintaining data consistency despite concurrent access. Our work explores a novel approach called CALock to synchronize operations on hierarchies by utilizing a labeling scheme that facilitates multi-granularity locking. Our approach addresses both concurrent data reads and writes as well as structural modifications. CALock exploits the hierarchical topology via a new labeling scheme to identify the common ancestors of vertices. This enables a thread to identify an appropriate lock granule for its lock request. Leveraging variable lock granularities optimizes operations across the hierarchy while ensuring consistency and performance. We provide a detailed discussion of the CALock labeling and the locking algorithm, prove its properties, and evaluate it experimentally. On static hierarchies CALock remains competitive with previous labeling schemes and has better concurrency and throughput when structural modifications change the hierarchy. In particular, CALock improves throughput by 4.5×, and response time by 1.5× for workloads that contain structural modifications. 
