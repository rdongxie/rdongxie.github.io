---
layout: post
title:  "学习erlang--fun的用法"
date:   2013-09-23 11:06:57
categories: erlang
---
  
#1.定义匿名函数

在Eralng中，同一个模块中的两个函数，如果她们同名但是它们的目（arity）不同，这样的两个函数被认为是完全不同的两个函数。   通常情况下，这样的函数被用作辅助函数。 fun函数就是一个匿名函数（因为他自己没有名字），但就这个匿名函数，用处却是很大的。
fun既可以作为函数的参数，也可以作为函数（或者自己本身fun）的返回结果。fun函数的简单使用：

	4> Triple = fun(X) -> 3*X end.
	#Fun<erl_eval.6.13229925>
	5> Triple(3).
	9
	6>


说明：#Fun<erl_eval...>的信息是Erlang标准库中erl_eval模块产生的函数信息,Fun代表的类型，不是变量。  
详细信息在以后分析stdlib的分档中给出

Triple = fun(X) -> 3*X end.  
是定义一个fun，只有一个参数量，也就是只有一个目，在fun结束的时候，需要将end加在后面的。  当我们要调用它的时候，直接使用Triple，然后加上参数就行了。


#2.fun函数的参数
	1> Hypot = fun(X, Y) -> math:sqrt(X*X + Y*Y) end.
	#Fun<erl_eval.12.113037538>
	2> Hypot(3, 4).
	5.0
	3>
其中的math是一个模块，sqrt是其中的一个函数，是用来计算平方根的。fun也可以有若干个不同的子句。
比如下面的星期转换:

	3> Week = fun({monday}) -> {1};
	3> ({tuesday}) -> {2}
	3> end.
	#Fun<erl_eval.6.13229925>
	5> Week({monday}).
	{1}
	6>
2.以fun作为参数的函数
lists是标准库中的一个模块，从中导出的很多函数都是以fun作为参数的函数，比如，map，filter等。

	1> L = [1, 2, 3, 4].
	[1,2,3,4]
	2> Double = fun(X) -> 2*X end.
	#Fun<erl_eval.6.13229925>
	3> lists:map(Double, L).
	[2,4,6,8]
	4> Even = fun(X) -> (X rem 2) =:= 0 end.
	#Fun<erl_eval.6.13229925>
	5> lists:filter(Even,L).
	[2,4]
	6> Even(8).
	true
	7> Even(7).
	false
	8> L.
	[1,2,3,4]
	9>



#作为参数用法2
spawn(fun -> loop/0)和spawn(loop())是等价的。


	-module(area_server_final).
	-export([start/0, area/1]).	

	loop() ->
		receive
			{From, {rectangle, Width, Ht}} -> From ! {self(), Width * Ht}, loop();
			{From, {circle, R}} -> From ! {self(), 3.1415926 * R * R}, loop();
			{From, Other} -> From ! {self(), {error, Other}}, loop()
		end.	

	start() -> Pid = spawn(fun -> loop/0), register(server, Pid), Pid.	

	area(What) ->
		Pid = whereis(server),
		Pid ! {self(), What},
		receive
			{Pid, Response} -> Response		
		end.

#3.使用fun进行类型预定义定义

	-spec filter(Pred, List1) -> List2 when
	      Pred :: fun((Elem :: T) -> boolean()),
	      List1 :: [T],
	      List2 :: [T],
	      T :: term().

#4.使用fun进行抽象流程定义
Erlang没有循环，抽象一个lib_misc.erl
	
	-module(lib_misc).
	-export(for/3).	

	for(Max, Max, F) -> [F(Max)];
	for(I, Max, F) -> [F(I)|for(I+1, Max, F)].

对这个例子而言，计算 for(1,10,F) 就是建立一个列表 [F(1), F(2), ..., F(10)].

#参考资料
http://erlang.group.iteye.com/group/wiki/2009-erlang_type_and_spec  