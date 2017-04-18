---
title:  "Concurrency in Erlang-Single Core Systems"
date:  "2017-04-18 16:43:13"
categories:  [erlang]
tags: [erlang]
---

*"In computer science, concurrency is the decomposability property of a program, algorithm, or problem into order-independent or partially-ordered components or units." -concurrency as defined by wikipedia.*



In Erlang concurrency is acheived by running processes independently, potentially running in parallel. In single core systems concurrent process time share, mediated by a Scheduler, in a round robin fashion. We have a 3 state-process model in Erlang, they are RUNNING, WAITING, RUNNABLE.

WAITING: A process in said to be in a WAITING state if it is in a receive statement and is waiting for some message to arrive.

RUNNABLE: A message is recieved or the timer of the process reaches zero, the process becomes RUNNABLE. This process id added to the end of the run queue. This run queue contains all the RUNNABLE processes.

RUNNING: When a process is sheduled out of the run queue, the process at the front of the time queue starts running.

<h3>DESCHEDULING:</h3>

Now the running process are descheduled after 1000 reduction steps, when 1000 reduction steps are reached the process is placed at the end of the run queue.The process can also get descheduled if it enters a waiting state with a empty mailbox and becomes waiting.

All RUNNABLES are scheduled in Round Robin.
