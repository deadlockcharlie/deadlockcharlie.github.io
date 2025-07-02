+++
title =  "Coordination avoidance in Database Systems"
date = 2025-07-01
tags = ["distributed systems", "consistency", "Invariant preservation", "Static Analysis"]
author = "Ayush Pandey"
+++
<!--more-->

Minimising coordination in database systems leads to higher performance. Coordination-free execution is not always safe. There may be operations which can compromise application level correctness. For example, un-coordinated withdrawl operations from a back account can cause the account balance to go negative. The paper by [Bailis et. al.](https://dl.acm.org/doi/pdf/10.14778/2735508.2735509) discusses the critera of reducing the need for coordination without sacrificing application level correctness. Their criteria is called $I$-confluence. It captures the potential scalability and availability of an application, independent of any particular database implementation. If the operations of a database are $I$-confluent, they can be executed without any coordination.

## System Model

A __Database__ is represented as a shares sey of $D$ unique versions, located on an arbitrary set of servers. Each version is located on atleast one server. 

