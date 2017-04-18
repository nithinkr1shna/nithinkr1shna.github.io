---
title:  "Concurrency in Erlang"
date:  "2017-04-18 16:43:13"
categories:  [erlang]
tags: [erlang]
---

*"In computer science, concurrency is the decomposability property of a program, algorithm, or problem into order-independent or partially-ordered components or units." -concurrency as defined by wikipedia.*



In Erlang, concurrency is acheived by running processes independently, potentially running in parallel. In single core systems concurrent process time share, mediated by a Scheduler, in a round robin fashion. We have a 3 state-process model in Erlang, they are RUNNING, WAITING, RUNNABLE.

WAITING: A process in said to be in a WAITING state if it is in a receive statement and is waiting for some message to arrive.

RUNNABLE: A message is recieved or the timer of the process reaches zero, the process becomes RUNNABLE. This process id added to the end of the run queue. This run queue contains all the RUNNABLE processes.

RUNNING: When a process is sheduled out of the run queue, the process at the front of the run queue starts running.

<h3>DESCHEDULING:</h3>

The running process are descheduled after 1000 reduction steps, when 1000 reduction steps are reached the current running process is placed at the end of the run queue.The process can also get descheduled if it enters a waiting state with an empty mailbox and becomes waiting.

All RUNNABLES are scheduled in Round Robin.

What happens in multi-core systems? In multicore BEAM(Erlang virtual machine) will have multiple run queues(default: one per core). The processes are scheduled on the same core that spawned it. This doesn't mean that a process cant be served on a differnt core. With the help of a work-stealing algorithm, the processes can be migrated among cores. This ensures that no core time is wasted.


<h3>Processes</h3>
In Erlang, a process can be created with the help of a spawn primitive.

```erlang
spawn(Module, Function, Arguments)
```
This creates a new process, we can get the process ID of the newly created process with

```erlang
Pid = spawn(Module,Function, Arguments)
```
we can also register names for a process. In general a single  module has one named process.

```erlang
register(foo, spawn(foo,init,[]))
```
Here we are creating a process and naming it as foo, which is the same name as the module(convention), init as function and empty arguments. By naming the process we can send messages to the process foo.

Message to a process is send with the help of bang(!) symbol. An example is shown below

```erlang

-module(test).
-export(start/0).

start()->
   register(test,Pid=spawn(test,run,[]))),
   {Pid,started},
run()->
   receive
     stop-> {stopped};
	 _Msg ->io:format("got ~s~n",[Msg]),
	        run()
   end.
   ```
 
In shell,

```erlang

1>c(test).
{ok,test}
2> test:start().
{<0.65.0>,started}
3> test ! hello.
got hello
hello
4> test ! stop.
{stopped}

```
Here we can see a receive end block in the run function, this statement is responsible for receiving the message send to that process. In the above example we created a thread, which receives a msg, displays it until stop atom is received. The concept of message well goes with mai box. In Erlang all the message send to a process will be placed into a process's mail box. The recieve statement is responsible for taking each of the messages in order as they are sent.

Cont....
