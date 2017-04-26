---
title: "Erlang: My First functional programming Language"
date: "2017-04-26 11:34:13"
categories: [erlang]
tags: [erlang]
---

Erlang is a general purpose, concurrent functional programming language.I have attended two online courses from futurelearn[futurelearn_link]. The first course mainly focussed on the basic of the Erlang Language and the second course was into concurrent programming in Erlang. The Erland Language is approx 30years old and getting popularity these days due to its wide usage in concurrent environment.

The Erlang was orginally designed for improving the development in telephony applications, where thery have to handle a lot of active connections at the same time. Another problem that lead to the development of Erlang is the need for availability and fault tolerance. By 1988 Erlang proved that it was suitable for telephony applications. In 1992 the work for erlang virtual machine BEAM was started( earlier versions of erlang was implemented in Prolog which was found slower hence BEAM). In 1998 Ericsson announced they were able to acheive high availability of nine 9's using Erlang circuit switched digital telephone exhanges manufactured by Ericsson.



<h3>SOME OF THE FEATURE OF ERLANG (JOE ARMSTRONG)</h3>
<ul>
<li>In Erlang everything is a process.</li>
<li>Process are strongly isolated.</li>
<li>Process creation and destruction are light weight.</li>
<li>Message passing is the only way for processes to interact.</li>
<li>Process has unique names.</li>
<li>If you know the nme of process you can send message to it.</li>
<li>Processes share no resources.</li>
<li>Error-handling is non-local.</li>
<li>Processes do what they are supposed to do else they fail.</li>
</ul>
[from wiki][wikipedia_link_features]


With all this in mind lets move forward and allow me to write the challenges faced and the new experiences gained during the said courses.


<h3>Course 1: Basic Programming In Erlang.</h3>

<h4>Week 1:</h4>

  The first week of the course was mainly focussed on Erlang basics. One of the interesting aspect of Erlang that I came to know is the Pattern Matching. In Erlang variables are bind to values using pattern matching, uses single assignment. For example 

{% highlight erlang %}

Eshell V8.2  (abort with ^G)
1> A=5.
5
2> A.
5
3> A=6.
** exception error: no match of right hand side value 6
{% endhighlight %}


  In the above example , A is bounded to value 5. If we try to bind it again(step 3) we will get an exception. Another thing to note is that the varibales must start with Capital letter otherwise it will become an atom. [Atoms][atoms_link_stackoverflow] are immutables, a literal ,a constant with a name.

  Functions:
    A simple function can be defined like this,

{% highlight erlang %}

-module(isZero).
-export([is_zero/1]).

is_zero(0)->
 true;
is_zero(X)->
 false.
 {% endhighlight %}
 
   The function isZero checks whether a given value is zero or not. If first clause holds we will get true else false. The clauses are separated by a semi-colon(;) and the final clause have a period(.) at the end. We have a module name isZero and a export list. In export list is_zero/1 says that is_zero is a function which will take a single argument. Every  function should be exported to use outside the module. The function vcan be executed after compiling the file isZero.erl(same as the module name) as shown below. This function in action,


{% highlight erlang %}
    
1>c(isZero).  %% Compiling
{ok, isZero}
2> isZero:is_zero(0). %% Accessing module:function/arity
true
3> isZero:is_zero(3).
false

{% endhighlight %}

Recursion
      Another different thing I saw in Erlang was that there is no explicit control structures like for , while loops etc. We have to attain looping with recursion alone. An example of factorial problem,

{% highlight erlang %}
    
fact(0)->
    1;
fact(N)->
    fact(N-1)*N.

{% endhighlight %}

we can implement this with Tail Recursion as



{% highlight erlang %}

fact(N)->
    fac(N,1).

fac(0, Acc)->
    Acc;
fac(N, Acc) when N > 0 ->
    fac(N-1, Acc*N).

{% endhighlight %}


   Difference between Recursion & Tail Recursion [go_here][learn_you_some_erlang_link_recursion]. This Article explains tail recursion in a fantastic way.
    
A while loop implementation:

{% highlight c %}
    counter =5;
    while(counter > 0){
        printf("%d\n",counter);
    	counter--;
    }
	
{% endhighlight %}

we can implement the same with Erlang like,

{% highlight erlang %}

while_loop(N) when N > 0 ->
    io:format("~p~n",[N]),
    while_loop(N-1).
	  
{% endhighlight %}
	
<h4>Week 2</h4>

   Second week on the course was about Lists. List is a collection of Items, where you can mix more than one [data types][erlang_doc_datatypes_link].
   Ex: [1,2,3,4,5],
       ["string",atom,1,[1,2,3],{tuple, 1}]
       [] <- empty list

   We can match a list like this

{% highlight erlang %}
    
   Eshell V8.2  (abort with ^G)

   1> List = [1,2,3,4,5].
   [1,2,3,4,5]
   2> [X|Xs] = List.
   [1,2,3,4,5]
   3> X.           %% the head part.
   1
   4> Xs.          %% the tail part.
   [2,3,4,5]
   5> [L,M|Ms] = List.
   [1,2,3,4,5]
   6> L.           %% first element.
   1
   7> M.           %% second element.
   2
   8> Ms.          %% and the rest.
   [3,4,5]

{% endhighlight %}
   [more on lists][erlang_doc_man_lists]. 


An example on list, sum of the elements of a list

{% highlight erlang %}

   sum(N)->
     sum_list(N,0).

   sum_list([],Acc)->
       Acc;
   sum_list([X|Xs], Acc)->
      sum_list(Xs, Acc+X).

{% endhighlight %}

   In Erlang clauses are scanned sequently until a match is found.

<h4>Week 3</h4>
   Higher Order Functions:
   
   Some of the widely used higher order functions in Erlang are lists:map/2, lists:foldl/3, lists:filter/2 or lists:filtermap/2 etc. The foldl or foldr reduces the list into a single value, this can be used to find the sum of elements in a list. The function map, maps all element in the list. The filter selects all the elements in the list with a particular property. Examples,

{% highlight erlang %}

   Eshell V8.2  (abort with ^G)
  
   1> List =[1,2,3,4,5].
   [1,2,3,4,5]
   2> Fun  = fun(X,Sum) -> Sum+X end.
   #Fun<erl_eval.12.52032458>
   3> Result = lists:foldl(Fun, 0, List). %% foldl(Function, Acc0, List)
   15
   
{% endhighlight %}
   
   The foldl/3 function takes a function Fn , Acc0 and a list. Fun(Elem, AccIn) on successive elements A of List, starting with AccIn == Acc0. Fun/2 must return a new accumulator, which is passed to the next call. The function returns the final value of the accumulator. Acc0 is returned if the list is empty.

{% highlight erlang %}

Eshell V8.2  (abort with ^G)
  
1> List =[1,2,3,4,5].
[1,2,3,4,5]
2> Fun = fun(X) -> X*X end.
#Fun<erl_eval.6.52032458>
3> Result = lists:map(Fun, List).
[1,4,9,16,25]

{% endhighlight %}


The lists:map/2 function maps each element. The above example find the square of all elements in a list. 

{% highlight erlang %}

   1>lists:filter(fun(X) -> X rem 2 /= 0 end, [1,2,3,4,5,6,7,8,9,0]).
   [1,3,5,7,9]

{% endhighlight %}

  The lists:filter function filters out the odd numbers from the given List. [more on lists][erlang_doc_man_lists]


And at the end of the course we had an assignment. The assignment was to implement rock-paper-scissors. 


<h3>Course 2: Concurrent Programming in Erlang</h3>
<h4> Week 1</h4>

The main feature of Erlang that discussed in the second course was the concurrency in Erlang. In this section we were introduced with the concept of message passing. The only way a process could interact with another process is through this message passing. This message passing can be synchronous or asynchronous.

In Erlang both concurrency and parallelism can be acheived. On a single processing element, the concurrent process time share([more][concurrency_my_notes]). We can spawn a process with the help of [spawn][spawn_erlang_doc_link] primitive. On a [multicore processor][multicore_concurrency_my_notes] parallelism can be acheived. A process is a separate computation, running on its own space, time sharing with other [process][a_whole_lot_info_on_process].

Consider this 

{% highlight erlang %}

  area({square, X})-> %% pattern
      X*X;            %% action for that above pattern
  area({rectangle, X, Y})->
      X*Y.

{% endhighlight %}

we can convert the above sequential code to concurrent like,

{% highlight erlang %}

-module(area_mod).
-export([area/0]).

area()->
    receive
       {square, X}->
            X*X;
       {rectangle, X, Y}->
            X*Y
    end.

{% endhighlight %}

This can now be run from the shell 


{% highlight erlang %}

1>c(area_mod).
{ok, area_mod}
2>Pid = spawn(area_mod, area,[]).
<0.65.0, ok>
3> Pid ! {square, 5}.
10
4> Pid ! {rectangle, 3,4}.
12

{% endhighlight %}

Here we have spawned a process with the spawn primitive and send message {square, 5} to it. It will be send to the process's(<0.65.0>) mail box and from there it will be handled by the receive statement.

![process-mail-box](/assets/msg-passing-btwn-processes.png)

If we send a message that wont match any pattern in the receive statement, those messages will reside in the mailbox which can be flushed with the help of [flush/0][flush_explained_in stackoverflow]. 

We were introduced with a the idea of a frequency server, where a server is responsible for alloting frequency to clients. So the basic functionality of the server is like allocating frequency to the clients and deallocating the alloacted frequencies. This is implemented with the help of a pair of lists {Free, Allocated}, and a function to allocate frequencies and a function to deallocate frequencies. Both of these function return new states.  We were able to implement this like,

{% highlight erlang %}

1>c(frequency).
{ok, frequency}
2> frequency:allocate().
{ok, 10} %% 10 is the allocated freqeuncy.
3> frequency:allocate().
{ok, 11} %% 11 is allocated frequency.
4> frequency:deallocate(11).
ok
5> NewFreq = [25, 26, 27, 28, 30].
[25, 26, 27, 28, 29, 30]
6> frequency:inject(NewFreq).
ok.     %% new frequencies added to the list of free frequencies.

{% endhighlight %}

<h4>Week 2</h4>
 
 Hot code Loading in Erlang is responsible for six Nine's avalability. That is Erlang production systems are available 99.9999 % of time. By this method we were able to add new freqeuncies to already running server. This implies that the production system was online during the upgradation. 

A design philosophy in Erlang is "Let it fail", this means that if a process fail , let it fail. A process can either execute indefinetly, terminate normally or fail. When a process fails system integrity is destroyed, a message can be sent to a non existent process or will wait for a message from a failed process. So this has to be dealt. Linking is the method of linking process together, so that if a process fails all the process linked to it directly or indiretly will get an error signal(when this signal is received by a process , its default behaviour is self termination). But this can be avoided by converting this signal to message by trapping exit. This is done by setting the process_flag to true, 

{% highlight erlang %}
 process_flag(trap_exit,true)
 `{% endhighlight %}
 
Then this will be transformed to

{% highlight erlang %}
{'EXIT',FromPid,Reason} %% this is put into the mailbox, just like any regular message.
{% endhighlight %}

[Links][link_vs_monitors_erlang] are bidirectional and [monitors][link_vs_monitors_erlang] are unidirectional.

Supervisors are responsible for starting, stopping and monitioring its child processes. The basic idea is that the supervisor must keep its child process  alive by restarting them as necessary according to some strategy.[more][supervisor_behavior_erlang_doc_man_link]. 

![supervision-tree](/assets/supervisor.png)

Erlang also supports [Exception][exception_handling] handling, throw and catch. Some of the errors that can occur include badmatch, badarith, undef, case clause etc.



<h4>Week 3</h4>

In [multicore][multicore_concurrency_my_notes] systems Erlang acheives true parallelism. The processes are scheduled per core amd each of core has separate run queues. And because of this multiple process can be active simultaneously one on each core. In multicore sequential computations can be done in parallel. We were asked to scale the frequency server with two servers each of them handling half of the frequencies. We were able to implement this by creating a routing process which checks which server  has less load at that particular time and that server will handle the freqeuncy allocation request.

[more on multicore erlang][multicore_concurrency_my_notes]





[atoms_link_stackoverflow]:  http://stackoverflow.com/questions/36023947/how-do-erlang-atoms-work/36025280
[wikipedia_link_features]:  https://en.wikipedia.org/wiki/Erlang_(programming_language)#Erlang_Worldview
[learn_you_some_erlang_link_recursion]:  http://learnyousomeerlang.com/recursion
[erlang_doc_datatypes_link]: http://erlang.org/doc/reference_manual/data_types.html
[erlang_doc_man_lists]:  http://erlang.org/doc/man/lists.html
[concurrency_my_notes]:  https://nitkna.github.io/2017/concurrency-in-erlang/
[spawn_erlang_doc_link]: http://erlang.org/doc/man/erlang.html#spawn-2
[multicore_concurrency_my_notes]: https://nitkna.github.io/2017/concurrency-in-erlang-part2/
[a_whole_lot_info_on_process]: http://erlang.org/doc/reference_manual/processes.html
[flush_explained_in_stackoverflow]: http://stackoverflow.com/questions/11989627/empty-process-mail-box-in-erlang
[supervisor_behavior_erlang_doc_man_link]: http://erlang.org/doc/man/supervisor.html
[link_vs_monitors_erlang]: http://marcelog.github.io/articles/erlang_link_vs_monitor_difference.html
[exception_handling]: http://learnyousomeerlang.com/errors-and-exceptions
