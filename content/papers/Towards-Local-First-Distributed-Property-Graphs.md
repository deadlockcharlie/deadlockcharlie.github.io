---
title: "Towards Local-First Distributed Property Graphs"
date: 2025-03-01
venue: '12th Workshop on Principles and Practice of Consistency for Distributed Data (PaPoC)'
paperurl: 'https://hal.sorbonne-universite.fr/hal-04985261'
citation: ' Ayush Pandey,  Stefania Dumbrava,  Marc Shapiro,  Carla Ferreira,  M{\&apos;a}rio Pereira,  Nuno Pregui{\c c}a, &quot;Towards Local-First Distributed Property Graphs.&quot; 12th Workshop on Principles and Practice of Consistency for Distributed Data (PaPoC), 2025.'
---

Summary
------
A graph database system (GDBMS) is designed to efficiently manage highly interconnected and diverse data. Compared to a traditional database system, a GDBMS has more stringent requirements due to long running transactions which also have a large footprint as they access a large number of vertices, as well as specific constraints and invariants related to graphs, such as key and connectivity constraints. Hence, existing sharding and georeplication techniques for scalability and availability are not well-suited for GDBMSes. Strong isolation levels suffer from a high probability of spurious conflict and unavailability, while techniques for highly available isolation, such as Replicated Data Types (RDTs), are well studied for traditional key-value stores but largely unexplored for GDBMSes. We analyze the interplay between availability, consistency, and constraints and propose studying RDTs and consistency levels tailored for GDBMSes.

[Access paper here](https://hal.sorbonne-universite.fr/hal-04985261)
