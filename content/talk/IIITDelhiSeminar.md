+++
title= "Formalising the foundation-Modelling and Verifying a Database Backend"
venue= "IIIT Delhi"
date= 2025-07-11
location= "Online"
+++

This talk was given as a presentation for a Seminar at [IIIT Delhi](https://www.iiitd.ac.in). 

## Abstract

Although a database backing store is conceptually just a shared memory, modern stores have high
internal complexity, for performance, fault tolerance, and to support rich data types. As such 
complexity is bug-prone, our work proposes a correct-by-design approach, based on a set of
formal specifications. Possible executions are abstracted as a trace, a partial order of transactional
events. Its valuation specifies the expected value of a key at each event, according to data type
semantics. We specify two common variants of store, the eager, random-access map and the lazy,
sequential-access journal. We show that both conform to the valuation, meaning that they are 
(i) safe, (ii) functionally equivalent, and (iii) causally consistent. Finally, we introduce a new timestamp
inversion anomaly, and provide a sufficient condition to avoid it.

[slides PDF](/talk/slides/IIITDelhiSeminar.pdf)