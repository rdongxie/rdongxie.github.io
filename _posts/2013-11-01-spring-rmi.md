---
layout: post
title:  "Spring RMI使用注意点"
date:   2013-11-01 09:36
categories: python
---
spring rmi 

1.日志级别设置为debug以上一别  
2.registHost设置后不会创建注册中心，仅仅会寻找注册中心，寻找不到就失败。 
3.在tomcat中启动服务，要把registry由spring创建，参加(http://forum.spring.io/forum/spring-projects/integration/jms/24244-eofexception-using-rmi-remoting)  
4. 设置RMI属性(http://docs.oracle.com/javase/7/docs/technotes/guides/rmi/javarmiproperties.html)  
