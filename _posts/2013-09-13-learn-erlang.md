---
layout: post
title:  "学习erlang"
date:   2013-09-13 11:06:57
categories: erlang
---

# 1.使用图书
<img src="/img/programming-erlang.bmp" />

# 2.第二章
注释：单行注释,%后面的一行都是注释会被erlang忽略  

	% just a comment...

在erlang的shell中可以使用ctrl+p和ctrl+n切换之前(后)的输入

## erlang shell使用

## 变量
_是一个匿名变量，真正的变量。
以大写字母开头都是变量？

## 2.7浮点数

	5/3. %1.66667  
	4/2. %2.00000  
	5 div 3. %1
	5 rem 3. %2  
	5 div 2. %2  


## 2.8原子
ruby中的symbol吗？  
C语言中的常量？  
原子是一串以小写字母开头，后面跟数字、字母(大小写都可以)、下划线、邮件符号的字符。
原子可以包含其他字符吗？原子可以以大写字母开头吗？  
答案：使用单引号，'Monday','Tuesday','+','*','an atom with spaces'

## 2.9元组
python中的元组？ruby中的元组？
_  特殊变量
{}

##2.10列表
python中的列表？ruby中的列表？
[]

##2.11字符串
12

##2.12 模式匹配
<img src="/img/pattern-match.bmp" />

#3.第三章--顺序型编程
##3.1模块
##3.2购物函数
##3.3同名不同目的函数
##3.4fun
##3.5简单的列表处理
##3.6列表解析
###3.6.1快速排序
<a href="http://baike.baidu.com/view/19016.htm">快速排序</a>
###3.6.2毕达哥拉斯三元组
###3.6.3变位词
递归
##3.7算术表达式
<img src="/img/er-operation.jpg">
##3.8断言
###3.8.1断言序列
###3.8.2断言样例

	f(X) when (X==0) or (1/X > 2) -> %X=0 false
	g(X) when (X==0) orelse (1/X > 2) ->%X=0 true

###3.8.3true断言的使用
###3.8.4过时的断言函数
##3.9记录
###3.9.4记录只是元组的伪装

##3.10case/if表达式
###3.10.1case表达式

	case Expression of
		Pattern1 [when Guard1] -> Expr_seq1;
		Pattern2 [when Gurad2] -> Expr_seq2;
	end

	filter(P, [H|T]) -> filter1(P(H),H,P,T);
	filter(p, []) ->[].
	filter1(true, H, P, T) -> [H | filter(P, T)];
	filter1(false, H, P, T) -> filter(P, T).

	filter(P, [H | T]) -> 
		case P(H) of
			true  -> [H | filter(P, T)];
			false -> filter(P, T)
		end;
	filter(P,[]) -> [].

###3.10.2 if表达式

	if
		Guard1 -> Expr_seq1;
		Guard2 -> Expr_seq2;
		true -> Expr_seq3; %best practice,
	end.

##3.11以自然顺序创建列表
真心不明白这节说的什么玩意，就知道要反转一个列表要调用lists:reverse/1

##3.12累加器

#4. 异常
##4.1 异常
	cost(apples) -> 3;
	cost(oranges) -> 8;
	cost(newspaper) -> 9.

	cost(stock).

这个时候会抛出异常，cost函数没有必要处理异常，因为cost不知道如何处理，所以直接抛出

抛出异常：
1.系统内部错误  
2.在代码中显示调用throw(Exception)、exit(Exception)、erlang:error(Exception)

捕获异常：  
1.用try...catch将一个会抛出异常的函数括起来  
2.把函数调用包含在catch表达式里.  

##4.2抛出异常
系统内部错误  
exit()  
throw()  
erlang:error()  

##4.3try...catch

##4.4 catch
dont understand totally

##4.5 


#7.并发
全是描述erlang并发原理(对真实世界并发的抽象)的文字。  
一个erlang程序使用很多进程组成，这些进程没有共享内存，进程(这里的进程是erlang的概念，并不是操作系统的概念)间的交流都是通过  
消息来传递，并且发送消息的进程并不知道其他进程是否已经收到消息，必须发送消息去询问。  当一个进程消亡时它会广播给和它有关系的进程。并且把消亡原因告诉这些有  
关系的进程。 当然进程也可以产生子进程，这个对应的专业术语是spawn。  
messages  
pids  
send !  
recive
spawn  

#8.并发编程
##8.1并发原语
	Pid = spawn(Fun)  
	Pid1!Pid2!Pid3!Pid4!...!M  
	recive  
	Pattern1 [Guard1] ->
	Expression1  
	Pattern2 [Guard2] ->
	Expression2
	...
end.
##8.2简单样例
	
	-module(area_server0).
	-export([loop/0]).
	loop() ->
		recive
			{rectangle, Width, Ht} ->
				io:format("Area of rectangle is ~p~n",[Width, Ht]),
				loop();
			{circle, R} ->
				io:format("Area of circle is ~p~n", [3.14159 * R * R]),
				loop();
			Other ->
				io:format("i dont know what the area of a ~p is ~n", [Other]),
				loop()
		end.

http://www.oschina.net/news/44153/try-10-programming-languages-in-10-minutes  
http://www.google.com.hk/search?newwindow=1&safe=strict&biw=1440&bih=762&noj=1&q=iops&oq=iops&gs_l=serp.3...122466.124056.0.124225.4.4.0.0.0.0.0.0..0.0....0...1c.1.26.serp..4.0.0.oLZrqqKsjdI  
http://blog.sina.com.cn/s/blog_75f0b54d0100r7af.html  
http://www.cnblogs.com/lawnight/archive/2012/08/09/2505995.html  
http://baike.baidu.com/view/1250961.htm  
http://www.cnblogs.com/zhyg6516/archive/2011/01/22/1941809.html  
http://product.dangdang.com/20671753.html  
http://product.dangdang.com/20046247.html  
http://search.dangdang.com/?key=linux%20%C4%DA%BA%CB  
http://product.dangdang.com/9112052.html#catalog  
http://blog.csdn.net/flyqwang/article/details/6418288  
http://os.51cto.com/art/201004/197184.htm  
http://timyang.net/programming/linux-synchronization-lock/  
http://www.google.com.hk/#newwindow=1&q=linux+java+%E9%94%81%E5%AE%9E%E7%8E%B0&safe=strict  
http://www.google.com.hk/#newwindow=1&q=atomic+cpu+%E9%94%81&safe=strict  
http://www.google.com.hk/search?newwindow=1&safe=strict&site=&source=hp&q=atomic+%E6%8C%87%E4%BB%A4&oq=atomic+%E6%8C%87%E4%BB%A4&gs_l=hp.3...1599.4778.0.5042.9.9.0.0.0.0.33.33.1.1.0....0...1c.1.26.hp..8.1.32.Jil7X4pDHfc&bav=on.2,or.&bvm=bv.52164340%2Cd.dGI%2Cpv.xjs.s.en_US.CQsooEYev9Y.O&biw=1440&bih=762&dpr=1&ech=1&psi=AHkyUpbHJYaMkwWV7ICoAQ.1379039484758.3&emsg=NCSR&noj=1&ei=AHkyUpbHJYaMkwWV7ICoAQ  
http://timyang.net/programming/linux-synchronization-lock/  
http://www.ibm.com/developerworks/cn/linux/l-cn-mcsspinlock/  
http://maoyidao.iteye.com/blog/1563523  
http://blog.csdn.net/fg2006/article/details/6397900  
http://wenku.baidu.com/view/a0104d39580216fc700afd90.html  
http://wenku.baidu.com/view/7275d5156edb6f1aff001f4b.html  
http://www.chineselinuxuniversity.net/articles/28072.shtml  
http://www.linuxidc.com/Linux/2012-01/51218.htm  
http://kenwublog.com/theory-of-java-biased-locking  
http://hzcsky.blog.51cto.com/1560073/502778  
http://ihacklog.com/post/nginx-reverse-proxy-setup.html  
http://blog.chinaunix.net/uid-52437-id-3064714.html  
http://liuyu.blog.51cto.com/183345/166381/  
http://www.2cto.com/os/201109/104591.html  
http://blog.csdn.net/bigtang5/article/details/1757676  
http://baike.baidu.com/view/19016.htm  
http://www.cnblogs.com/morewindows/archive/2011/08/13/2137415.html  
http://blog.csdn.net/sws9999/article/details/2791812  
http://student.zjzk.cn/course_ware/data_structure/web/paixu/paixu8.3.2.1.htm  
http://www.oschina.net/code/snippet_54100_8734  