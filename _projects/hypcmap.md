---
layout: post
title: HyPC-Map
description: A Hybrid Memory Parallel Infomap
---


A Hybrid Parallel Community Detection Algorithm Using Information-Theoretic Approach
============


The goal of the project was to design a parallel algorithm for information-theoretic community discovery (Infomap) to achieve faster computation for massive graph datasets while ensuring the correctness of the original sequential algorithm. To attain the goal, initially, we developed a distributed memory parallel implementation using MPI (message passing interface) framework.  We had to consider the following scenarios during the parallel algorithm design, i) entities in the graph belonging to the same group or community may be distributed across different parallel processes and need to be merged later to ensure the correctness of the formed community, ii) There might be repetitive activities due to the randomness of data distribution across different processes, iii) There might be situations requiring certain results to be computed first before applying or attributing community membership to a subset of entities (vertices in a network) and that results might not be available right away since another process with its private memory was working on that outcome, iv) there is a very high chance of workload imbalance due to the nature of the dataset despite distributed in equal number across different MPI processes.

All of the challenges mentioned above were crucial to maintaining the accuracy of the outcome of the discovered communities. We used problem-specific heuristics to deal with those challenges. The parallel algorithm design scaled up to 512 processing cores where each of the processes was dealing with a part of the dataset. The designed parallel algorithm demonstrated better parallel efficiency than state-of-the-art distributed techniques.

During empirical analysis, we found that synchronizing the outcomes of different processes at the end of each computational phase (aka iteration) was a performance bottleneck for the algorithm to reach better scalability and speedup. The communication cost is proportional to the number of distributed processes used by the parallel algorithm. To reduce the communication cost, we combined the OpenMP framework with the MPI framework in the algorithm design. Consequently, we achieved better scalability reaching up to 1280 processing cores and a 25X speedup than the state-of-the-art sequential Infomap algorithm and at least 4X better speedup than the state-of-the-art parallel distributed algorithm.
