---
title:  "My first Functional Programming Language "
date:   2017-03-26 10:45:23
categories: [erlang]
tags: [erlang]
---

Hai, this is going to be my first post. I think i will start this blog by narrating the experience with my first functional programming Language. Erlang! 
So what is Erlang, for me it is a functional programming language. But wikipedia defines Erlang as "Erlang is a general-purpose concurrent, functional programming language, as well as a garbage-collected runtime system."
Now coming to Erlang.

The last week I have attended a course in functional programming, a free course provided by www.futurelearn.com. The course tutor was Simon Thompson, Professor of Logic and Computation/ Director of Innovation, University of kent. The course duration was for 3 weeks. 
At first the concept was a little difficult for me but later on it becomes a lot more easier. One of the main difference I fond in Erlang is that there is no looping syntax as in c or other programming languages. For acheiving any looping we have to do recursion. 
For example: The factorial of a number

 In c:

``` c
 factorial(x); // function call
```

Then the function definition is like this.

``` c
 int factorial(int x){
  
  if(x==1)
   return 1;
  else
    return x*factorial(x-1);
 }

```
 In Erlang, this can be defined like,

``` erlang
 fact(0)->
   1;
 fact(X)->
   X*fact(X-1).
```

To execute a Erlang program we have the save this code in a file, say fact.erl(.erl for erlang). Next in the file we have to specify the name of the module, and should write a export list. For this example, this would be like,

-module(fact). 
-export([fact/1]).
 
module(fact) says that the name of this module is fact which should be same as that of the file name. Export list export([fact/1]) exports the fuction so that it can be called outside the module. In fact/1, fact is the name of the function and 1 is the arity (no of arguments). If there are more than one function to be exported then the list would be,

-export([fn1/arity_of_fn1,fn2/arity_of_fn2,......]).

The dot after any expression means an end. Another important thing in Erlang is that the order of execution is sequential.