---
title: "Concurrency in Erlang Part-2"
date: "2017-04-21 19:27:13"
categories: [erlang]
tags: [erlang]
---

For [Part 1][part-one-concurrency-in-erlang]

<h3>Concurrency In Multicore Systems</h3>

 What happens if the  BEAM is presented with a multicore System ?
 
In the multicore version of the BEAM Virtual Machine, each core has separate run queue. As said the BEAM will have run queues on each core, say if the system has 12 cores then there will be 12 run queues as default.The process are spawned on the same core where they are spawned. This processes can move between different cores with the help of a work-stealing algorithm.

![multicore-beam]({{site.url}}/assets/multicore-beam.png)
By Work-Stealing the process can move between different cores. This means that for a long lived process, it can essentially move between different cores a number of times. This ensures that maximum no of cores are utilized.

So it can be said that , if different process are to run on diferent cores this means that parallelism is obtained. 


[part-one-concurrency-in-erlang]: https://nitkna.github.io/2017/concurrency-in-erlang/
