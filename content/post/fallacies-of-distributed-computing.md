+++
title= 'Fallacies of distributed computing'
date= 2025-04-04
tags = ["distributed computing", "serverless computing"]
author = "Ayush Pandey"
description = "Fallacies of distributed computing"
+++
<!--more-->

Today while looking at some literature about serverless computing from RedHat, i came across a wikipedia article that mentions the fallacies of distributed computing. The original assertions were made by L Peter Deustch and others at Sun Microsystems. 

Having worked with distributed systems for a little while now, I think i have made all of these mistakes when thinking about a system. 

The fallacies are the following:

1. The network is reliable
2. Latency is zero
3. Bandwidth is infinite
4. The network is secure
5. Topology does not change
6. There is one administrator
7. Transport cost is zero
8. The network is homogenous

Throughout the years, there have been additions to this list. Mostly as system design paradigms have evolved. With serverless computing, there is three additional fallacies which are relevant. 

1. Versioning is simple 
2. Compensating transactions always work
3. Observability is optional


I am now exploring this domain of computing for future research challenges. More details soon. 