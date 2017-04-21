---
title: "Generic Server in Erlang"
date: 2017-04-21 20:34:36
categories: [erlang]
tags: [erlang]
---

NOTE: There is no introduction here

```erlang
gen_server:start_link({local, module},module,[],[]).

```

We can start the generic server with the call start_link which generally takes four arguments. The First argument specifies the name(here name is locally registered, it can also be globally registered).The second argument is the Module in which the callback fuctions reside. Fourth function is for the callback function init/1. Fifth argument is the list of options. That is 

Argument 1: Name of the generic server.
Argument 2: Name of call back Module.
Argument 3: Call back function init.
Argument 4: List of Options.

We can go through this concept with the help of a example. This example is of a Simple library(lib Module which has function interfaces) and corresponding generic server(gs Module). Here through the functional interfaces simple actions like view all the available books, lookup a book, checkout a book  are possible. 



<h4>Functional Interfaces</h4>


```erlang

-module(lib).


-export([start/0, lookup/1, add/1, checkout/1, record/0]).

start()->
    gen_server:start_link({local, ?MODULE}, gs, [], []).



lookup(Book)->
    {reply, Availabilty} = gen_server:call(?MODULE, {lookup, Book}),
     Availabilty.

checkout(Book)->
   {reply,Msg,_Books} = gen_server:call(?MODULE, {checkout, Book}),
   io:format("~w~n",[Msg]).

record()->
   [{Available, CheckedOut}] = gen_server:call(?MODULE, available_books),
   io:format("Available : ~w~n",[Available]),
   io:format("Checked Out : ~w~n",[CheckedOut]).
   
   
 ```
<h4>Callback Module</h4>

```erlang

-module(gs).
-behaviour(gen_server).

-export([handle_info/2,terminate/2,code_change/3]).

-export([handle_call/3,handle_cast/2,lookup/2,record/1,checkout/2,init/1]).

init(_Args)->
    {ok, {all_books(), []}}.



%%Hard coded

all_books() -> [1,2,3].

%%call_backs

handle_call({lookup, Book}, _From, Books)->
      Availability = lookup(Books, Book),
      {reply, Availability, Books};

handle_call(available_books, _From, Books) ->
    AllBooks = record(Books),
    {reply, AllBooks, Books};

handle_call({checkout, Book}, _From, Books) ->
    {reply,Msg, FreeBooks} = checkout(Books, Book),
    {reply,{reply,Msg, FreeBooks}, FreeBooks}.







%%Internals

lookup({Avbl_Books, _CheckedOut }, Book)->
    case lists:member(Book, Avbl_Books) of
	true -> {reply, available};
	false -> {reply, not_available}
    end.

record(Books)->
    [Books].

checkout({Avbl_Books, CheckedOut}, Book)->
    case lists:member(Book,Avbl_Books) of
     true ->
            Free_Books = lists:delete(Book, Avbl_Books),
	    {reply, book_checked_out, {Free_Books, [Book|CheckedOut] }};
     false ->
           case lists:member(Book, CheckedOut) of
           
              true -> 
		   {reply,book_already_checked_out,{Avbl_Books, CheckedOut}};
      	      false ->
                   {reply,book_not_in_lib,{Avbl_Books, CheckedOut}}

           end
    end.
	
handle_cast(_request, _State)->
    ok.

handle_info(_Info, State) ->
    {noreply, State}.

terminate(_Reason, _State) ->
    ok.

code_change(_OldVsn, State, _Extra) ->
    {ok, State}.


```
