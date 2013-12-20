---
layout: post
title:  "unix网络编程第五章第一个样例"
date:   2013-12-20 18:42
categories: unix socket
---

1.结构   
<img src="/img/chapter5.bmp" />


2.源码  

server.c

	#include "unp.h"
	#include <stdio.h>
	int main(int argc, char **argv)
	{
		int listenfd, connfd;
		pid_t childpid;
		socklen_t clilen;
		struct sockaddr_in cliaddr, servaddr;
		
		listenfd = Socket(AF_INET, SOCK_STREAM, 0);
		bzero(&servaddr, sizeof(servaddr));
		servaddr.sin_family = AF_INET;
		servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
		servaddr.sin_port = htons(SERV_PORT);
		bind(listenfd, (SA *)&servaddr, sizeof(servaddr));

		listen(listenfd, LISTENQ);
		for ( ; ;) {
			clilen = sizeof(cliaddr);
			connfd = Accept(listenfd, (SA *)&cliaddr, &clilen);
			if((childpid = fork()) == 0) {
				//Close(listenfd);
				printf("i am the server and i have accept your request,the message i return i will give it back\n");
				str_echo(connfd);
				exit(0);
			}
		Close(connfd);
		}
	}

client.c

#include "unp.h"

	int main(int argc, char **argv)
	{
		int sockfd;
		struct sockaddr_in servaddr;
		
		if (2 != argc) {
			err_quit("usage:tcpli <IP address>");
		}
		sockfd = Socket(AF_INET, SOCK_STREAM, 0);
		bzero(&servaddr, sizeof(servaddr));
		servaddr.sin_family = AF_INET;
		servaddr.sin_port = htons(SERV_PORT);
		Inet_pton(AF_INET, argv[1], &servaddr.sin_addr);
		Connect(sockfd, (SA *)&servaddr, sizeof(servaddr));
		str_cli(stdin, sockfd);
		exit(0);
	}

Makefile

	LIBS = -L/usr/lib/ -lunp
	objects = server.o 

	.PHONY all: tcpcli server

	tcpcli: client.o
		gcc client.o -o tcpcli $(LIBS)
	server: server.o
		gcc server.o -o server $(LIBS)
	server.o : server.c
		gcc -c server.c -o server.o

3.remarks:
http://blog.chinaunix.net/uid-26388452-id-3178098.html  
http://www.latelee.org/programming-under-linux/113-multi-makefile-for-app.html  
http://www.cnblogs.com/taskiller/archive/2012/12/14/2817650.html  
http://www.cnblogs.com/Anker/p/3242207.html  
http://www.oracle.com/technetwork/java/interceptingfilter-142169.html  
http://webfrogs.me/about/  
http://yonsm.net/about/  
http://blog.163.com/zhao_jinggui/blog/static/169620429201161163711467/  
http://wenku.baidu.com/view/069cdf650b1c59eef8c7b4b2.html  
http://www.cr173.com/html/19724_1.html  
http://blog.sina.com.cn/s/blog_605f5b4f0100x3wm.html  
http://blog.csdn.net/jiayouxjh/article/details/7602729  
http://hi.baidu.com/mgqw864/item/1194c9f56131430985d2787b    
