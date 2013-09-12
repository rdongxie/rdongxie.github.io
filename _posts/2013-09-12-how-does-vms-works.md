---
layout: post
title:  "虚拟机是如何工作的--(通用,Java,Ptyhon,Ruby等)"
date:   2013-09-09 22:06:57
categories: translation
---

# 1.背景 
在学习Python使用，突然想到，python没有使用字节码(以前，实际上现在python也已经使用字节码,*.pyc就是字节码)  
而java使用了字节码。那么在两者虚拟机启动时候，内存是如何初始化的，初始化的时候又加载了哪些内容呢？  
然后又想到虚拟机是如何把内部语言(python，java)翻译到机器指令的呢？

# 2.本地程序是如何工作的呢
首先我们要了解本地程序(就是非虚拟机运行的程序)是如何工作的呢?
首先了解一下计算机是如何工作的，这个问题还是留给大牛去解释吧  
程序计数器(program counter以下简称pc)：  
简单说一下，详细请看百科，pc中保存的是计算机下一条要执行的命令。  
那么这指令是如何从内存拷贝到pc中的呢？
cpu在执行IR(指令寄存器--存储cpu正在运行的指令)中命令后，会更改pc中的内容，若IR中的指令不是改变pc中的内容，则pc  
内容自动+ (一个指令长度)，指向下一个指令地址。若存在jmp和call 这2个指令则会修改pc中的指令地址。  
jmp为跳跃寻址，call是子进程(具体不详)
当程序启动时候，pc指向程序第一条指令，这样程序就可以运行下去了。

# 3.VM程序运行
当VM启动时候,(内核？)准备pc指向本程序第一条指令。运行VM的入口函数，VM解析脚本(这个时候pc一连续的+1操作--假设没有进行进程上下文切换)  
VM解析完脚本(java class中字节码中的jvm指令)生成机器指令，在内存充开辟一块区域存放这些机器指令，VM的最后一条指令是jmp跳转到生成的机器  
指令的地方，这样脚本就被执行了，这些VM解析这些脚本生成的最后一个指令是寻址指令，把pc内容重新指向VM程序的指令。


## 参考网址：
维基百科：http://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BC%8F%E8%A8%88%E6%95%B8%E5%99%A8  
百度百科：http://baike.baidu.com/view/178145.htm  
指令的跳跃寻址：http://share.onlinesjtu.com/mod/resource/view.php?id=527  
指令寄存器1：http://baike.baidu.com/link?url=FxfWLh1XMuudACwqmIpQniF0CNhIRFojPzmJFT0T2Ul3z8wQsJiW4d05vlTVcQ3A  
指令寄存器2：http://www.cppblog.com/Husiwa/archive/2010/12/15/136470.html  
指令寻址方式：http://course.cug.edu.cn/cugFirst/manage_theory/study/%E7%AC%AC%E5%9B%9B%E7%AB%A04.3.1.htm  
寻址方式： http://baike.baidu.com/view/889427.htm
各种寄存器： http://blog.sina.com.cn/s/blog_776847500100xjrz.html
学习：**http://share.onlinesjtu.com/mod/resource/view.php?id=527**

**
http://rdc.taobao.com/blog/cs/?tag=%E8%BF%9B%E7%A8%8B%E5%88%87%E6%8D%A2  
http://www.cnblogs.com/wz19860913/archive/2010/05/25/1742583.html  
http://wenku.baidu.com/view/b2c55da30029bd64783e2c1d.html  
http://www.google.com.hk/#newwindow=1&q=%E8%BF%9B%E7%A8%8B%E5%88%87%E6%8D%A2+%E5%AF%84%E5%AD%98%E5%99%A8&safe=strict  
http://wenku.baidu.com/view/b9b342ed172ded630b1cb67e.html  
http://qing.blog.sina.com.cn/2494474521/94aea919330006hz.html  
http://www.docin.com/p-20340421.html  
http://zhidao.baidu.com/link?url=1but-e7_ql1ixWm3Jq-JWE4Cyz0C5Sk44zHQpo37os17Zx8mxbJmEMODOspJUvQJvqkAuoQ6NR4Ixa7Dgqz1la  
http://thinkinmylife.iteye.com/blog/443900  
http://blog.csdn.net/hudashi/article/details/7062781  
http://diecui1202.iteye.com/category/117198  
http://book.51cto.com/art/201202/315865.htm  
http://book.51cto.com/art/201202/315868.htm  
http://baike.baidu.com/link?url=SEFxd-sIY-yyREYiP7y4R4X-4GWfTrIWrS18L9IHbYUFrUXT-fC3a2-wB_Zu_iktoltnhnyjbIDiVlizjag2Wa  
http://blog.chinaunix.net/uid-16459552-id-3364761.html  
http://englishman2008.blog.163.com/blog/static/28012907200810910551135/  
http://blog.csdn.net/tobyaries/article/details/8990869  
http://baike.baidu.com/view/2488819.htm  
http://baike.baidu.com/view/3047148.htm  
http://www.kerneltravel.net/journal/vi/syn.htm  

**


http://www.google.com.hk/#newwindow=1&q=%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E6%B1%87%E7%BC%96+%E5%9B%BE%E4%B9%A6&safe=strict  
http://baike.baidu.com/link?url=MniiWu527rGphPI_1S2dq_uEHD888twfkly8HGlBI1135hNN8anlKuUU7f9RXAdC4z2sikVSwNxG26BAJ4CRk_#3  
http://search.jd.com/Search?keyword=%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8F%8A%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80%E5%8E%9F%E7%90%86&enc=utf-8&book=y&area=12  
http://zhidao.baidu.com/link?url=lIcG3no6ty9QY0n4bn9ZNz5wgpQjCkKBWKUvMGxA26UV18tEoc806xdA0qqXghDT74mYZ3hFm2BXYFnY5ep-QK  
http://baike.baidu.com/link?url=cz_41u5mFP485ctwa_-vu4FR1kHGiPGGSsHSGIKHENQcXWl2oTc78SvmrcxKFFs862zg2NXZAV-vfQCvE48AiK  
http://wenku.baidu.com/view/458b4cf8fab069dc502201e2.html  
http://zhidao.baidu.com/link?url=cNFC3sovO7nKI9xOa9y2if_spoJSi_iW4JpQtyLSBDIg0sTjV5wj8_vLa6cXwyBlerXHnNfUUItb5XfHA3Yci_  



# 参考
1. http://hllvm.group.iteye.com/group/topic/33412  
http://hllvm.group.iteye.com/group/topic/33412  
http://hllvm.group.iteye.com/group/topic/17802  
http://rednaxelafx.iteye.com/category/29010  
http://rednaxelafx.iteye.com/blog/activities  
http://hllvm.group.iteye.com/  
http://rednaxelafx.iteye.com/blog/428780  
http://rednaxelafx.iteye.com/blog/428721  
http://hllvm.group.iteye.com/  
http://rednaxelafx.iteye.com/blog/1886170  
http://www.dewen.org/q/11202  

http://hi.baidu.com/smithallen/item/fa2b77e5438908c5bbf37db4  

http://www.javaworld.com/javaworld/jw-07-2008/jw-07-orm-comparison.html  
http://javapub.iteye.com/blog/751485  
http://wwty.iteye.com/blog/649171  
http://book.douban.com/subject/1134994/  
http://blog.csdn.net/skymingst/article/details/7436892  
http://book.51cto.com/art/201207/351014.htm  
http://ipseek.blog.51cto.com/1041109/795782  
http://south.readthedocs.org/en/latest/  
http://wenku.baidu.com/view/0af9427b01f69e314232940c.html  


jruby
http://wenku.baidu.com/view/189226114431b90d6c85c78c.html  
http://blog.csdn.net/yetyrain/article/details/5439186  
http://www.360doc.com/content/12/1216/01/9200790_254293908.shtml  
http://jruby.org/#1  
http://blog.jobbole.com/47619/  
http://blog.csdn.net/fyh2003/article/details/6837624  
http://wp.xdite.net/?p=3020  
http://wowubuntu.com/markdown/  
http://www.linuxidc.com/Linux/2012-06/62943.htm  

http://www.searchtb.com/2013/03/x86-64_register_and_function_frame.html  
http://blog.chinaunix.net/uid-16459552-id-3364761.html  
http://blog.csdn.net/w_shun/article/details/5612372  
http://zhidao.baidu.com/question/288304280.html  
http://www.cnblogs.com/mfm11111/archive/2009/03/29/1424300.html  
http://blog.csdn.net/w_shun/article/details/5572662  
http://bbs.csdn.net/topics/110101896  
http://blog.csdn.net/jjiss318/article/details/7185802  
http://www.lingcc.com/2012/05/16/12048/  
http://fm.zju.edu.cn/showCourse.php?id=66  
http://blog.sina.com.cn/s/blog_5d90e82f010184hx.html  

http://schemers.org/  
http://www.oschina.net/news/22321/learn-functional-programming  
http://www.oschina.net/news/33118/a-misconception-of-functional-programming  
http://www.oschina.net/news/12856/when-do-you-know-a-language  
http://www.oschina.net/news/38499/why-haskell-is-worth-learning  
http://blog.sina.com.cn/s/blog_5d90e82f010184hx.html  
http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-4.html#%_toc_%_chap_Temp_4  
http://www.oschina.net/news/27761/why-i-learn-haskell  
http://baike.baidu.com/view/1380355.htm  
http://www.erlang.org/download.html  
http://erlang-china.org/?page_id=119  
http://www.erlang.org/download.html  
http://blog.csdn.net/turingbooks/article/details/3247749  
http://www.haskell.org/haskellwiki/Haskell  
