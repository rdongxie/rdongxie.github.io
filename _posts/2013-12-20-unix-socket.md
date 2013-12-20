---
layout: post
title:  "转载--unix网络编程中unp.h的编程环境"
date:   2013-12-20 10:42
categories: unix socket
---
##1.概述  
最近在学习Unix网络编程（UNP），书中steven在处理网络编程时只用了一个#include “unp.h”  相当有个性并且也很便捷  
于是我把第三版的源代码编译实现了这个过程，算是一种个性化的开发环境的搭建吧，顺便把过程记录下来，以便自己以后查阅。  
首先去网上找到源代码包unpv.13e.tar.gz 一找一大堆解压缩到你的某个目录，unpv13e里面大致有这些目录  


	├── aclocal.m4 
	├── advio 
	├── bcast 
	├── config.guess 
	├── config.h 
	├── config.h.in 
	├── config.log 
	├── config.status 
	├── config.sub 
	├── configure 
	├── configure.in 
	├── debug 
	├── DISCLAIMER 
	├── icmpd 
	├── inetd 
	├── install-sh 
	├── intro 
	├── ioctl 
	├── ipopts 
	├── key 
	├── lib 
	├── libfree 
	├── libgai 
	├── libroute 
	├── libunp.a（就是为了生成这个文件） 
	├── Make.defines 
	├── Make.defines.in 
	├── Makefile 
	├── Makefile.in 
	├── mcast 
	├── mysdr 
	├── names 
	├── nonblock 
	├── oob 
	├── ping 
	├── README 
	├── route 
	├── rtt 
	├── sctp 
	├── select 
	├── server 
	├── sigio 
	├── sock 
	├── sockopt 
	├── sparc64-unknown-freebsd5.1 
	├── ssntp 
	├── streams 
	├── tcpcliserv 
	├── test 
	├── threads 
	├── traceroute 
	├── udpcksum 
	├── udpcliserv 
	├── unixdomain 
	├── unpv13e 
	└── VERSION


首先查看README 一般情况下我们只需要进行第一步和第二步 其他的是一些与其他架构有关的情况不管执行下面两部生成libunp.a  

	./configure
	cd lib 
	make

在lib上层目录中生成libunp.a，复制这个静态库到/usr/lib/和/usr/lib64/  中，因为后来编译程序的话需要用到这个静态库。还得在环境变量中将这两个路径加上。  

##2.配置unp.h

2.1.修改unp.h  
在unp.h中需要添加一行： 

	#define MAX_LINE 2048


2.2把unp.h和config.h放置在编译路径中
方法1：  

我在我的主目录下新建了一个unp目录，专门处理unp的例子。然后把lib下的unp.h和上层目录的config.h放入unp目录，然后在unp目录下新建各个要实践的程序的章节目录 比如一开头的time server例子我就新建了个time server目录，在里面写书中的例子程序
unp.h中将#include "../config.h"改成#include "config.h"

方法2：  

完成后即在在根目录中生成libunp.a文件。然后打开子目录lib下的unp.h文件。将里面的unp.h中将#include "../config.h"改成#include "config.h" ，如果已经是。则不用改。

拷贝文件。（需要有拷贝的权限，所以我是切换到root用户下去做的）  
a.将刚生成的libunp.a文件复制这个静态库到/usr/lib/和/usr/lib64/中  
b.将刚改过的unp.h文件和主目录的config.h文件拷贝到/usr/include中  


如果书写的程序出现err_sys()等err函数找不到的情况 这是因为steven大神对错误处理进行了封装,少了一个库文件apueerror.h网上找到后。  
将文件拷贝到/usr/include 中后。编译ok。  
下面将apueerror.h的代码复制出来。

 
	#include <errno.h>      /* for definition of errno */   
	#include <stdarg.h>     /* ISO C variable aruments */   
	  
	static void err_doit(int, int, const char *, va_list);  
	  
	/* 
	 * Nonfatal error related to a system call. 
	 * Print a message and return. 
	 */  
	void  
	err_ret(const char *fmt, ...)  
	{  
	    va_list     ap;  
	  
	    va_start(ap, fmt);  
	    err_doit(1, errno, fmt, ap);  
	    va_end(ap);  
	}  
	  
	  
	/* 
	 * Fatal error related to a system call. 
	 * Print a message and terminate. 
	 */  
	void  
	err_sys(const char *fmt, ...)  
	{  
	    va_list     ap;  
	  
	    va_start(ap, fmt);  
	    err_doit(1, errno, fmt, ap);  
	    va_end(ap);  
	    exit(1);  
	}  
	  
	  
	/* 
	 * Fatal error unrelated to a system call. 
	 * Error code passed as explict parameter. 
	 * Print a message and terminate. 
	 */  
	void  
	err_exit(int error, const char *fmt, ...)  
	{  
	    va_list     ap;  
	  
	    va_start(ap, fmt);  
	    err_doit(1, error, fmt, ap);  
	    va_end(ap);  
	    exit(1);  
	}  
	  
	  
	/* 
	 * Fatal error related to a system call. 
	 * Print a message, dump core, and terminate. 
	 */  
	void  
	err_dump(const char *fmt, ...)  
	{  
	    va_list     ap;  
	  
	    va_start(ap, fmt);  
	    err_doit(1, errno, fmt, ap);  
	    va_end(ap);  
	    abort();        /* dump core and terminate */  
	    exit(1);        /* shouldn't get here */  
	}  
	  
	  
	/* 
	 * Nonfatal error unrelated to a system call. 
	 * Print a message and return. 
	 */  
	void  
	err_msg(const char *fmt, ...)  
	{  
	    va_list     ap;  
	  
	    va_start(ap, fmt);  
	    err_doit(0, 0, fmt, ap);  
	    va_end(ap);  
	}  
	  
	  
	/* 
	 * Fatal error unrelated to a system call. 
	 * Print a message and terminate. 
	 */  
	void  
	err_quit(const char *fmt, ...)  
	{  
	    va_list     ap;  
	  
	    va_start(ap, fmt);  
	    err_doit(0, 0, fmt, ap);  
	    va_end(ap);  
	    exit(1);  
	}  
	  
	  
	/* 
	 * Print a message and return to caller. 
	 * Caller specifies "errnoflag". 
	 */  
	static void  
	err_doit(int errnoflag, int error, const char *fmt, va_list ap)  
	{  
	    char    buf[MAXLINE];  
	   vsnprintf(buf, MAXLINE, fmt, ap);  
	   if (errnoflag)  
	       snprintf(buf+strlen(buf), MAXLINE-strlen(buf), ": %s",  
	         strerror(error));  
	   strcat(buf, "/n");  
	   fflush(stdout);     /* in case stdout and stderr are the same */  
	   fputs(buf, stderr);  
	   fflush(NULL);       /* flushes all stdio output streams */  
	}  



3.版权声明
转载自一下文章的作者：     
[http://blog.csdn.net/lrh406317290/article/details/8501060](http://blog.csdn.net/lrh406317290/article/details/8501060)  
[http://www.cnblogs.com/shenlian/archive/2011/08/19/2146190.html](http://www.cnblogs.com/shenlian/archive/2011/08/19/2146190.html)  
[http://blog.163.com/hnhu_1987/blog/static/2000773042012111224039664/](http://blog.163.com/hnhu_1987/blog/static/2000773042012111224039664/)  

