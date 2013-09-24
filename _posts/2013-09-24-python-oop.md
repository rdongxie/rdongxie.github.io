---
layout: post
title:  "Python 面向对象编程"
date:   2013-09-23 11:06:57
categories: python
---

#1.经典类和新式类
目前学习的是Python2.x据说在Python3.0里面所有的类都将变成新式类。

在python的普通中类中，继承的模式是深度优先的，也就是说对于一个对象的搜索遵循从左至右、从下到上的原则，而这从下到上的原则又优于从左至右。
  
经典类继承:  

	# -*- coding:utf-8 -*-
	class A():
	    """
	    作为所有类的基类
	    """
	    value = 1
	     
	    def __init__(self, ):
	        """
	        """
	         
	class B(A):
	    """
	    D的基类，继承自A
	    """
	     
	    def __init__(self, ):
	        """
	        """
	 
	class C(A):
	    """
	    D的基类，继承自A
	    并且将基类中的变量值修改
	    """
	    value = 3
	     
	    def __init__(self, ):
	        """
	        """
	         
	class D(B, C):
	    """
	    """
	     
	    def __init__(self, ):
	        """
	        """
	         
	x = D()
	print x.value

对于以上的代码输出的结果将是1。  
因为对于经典类来说，在类D中调用对象value，首先要查找类D中是否有value，如果没有则按顺序查找B->A->  C。它是一种广度优先的查找方式，所以首先会在基类A中查找到对象value，然后输出1。  

新式类：

	# -*- coding:utf-8 -*-
	class A(object):
	    """
	    作为所有类的基类
	    """
	    value = 1
	     
	    def __init__(self, ):
	        """
	        """
	         
	class B(A):
	    """
	    D的基类，继承自A
	    """
	     
	    def __init__(self, ):
	        """
	        """
	 
	class C(A):
	    """
	    D的基类，继承自A
	    并且将基类中的变量值修改
	    """
	    value = 3
	     
	    def __init__(self, ):
	        """
	        """
	         
	class D(B, C):
	    """
	    """
	     
	    def __init__(self, ):
	        """
	        """
	         
	x = D()
	print x.value

对于这个例子输出的结果将是3。  

因为基类A继承于object，而所有继承于object的类都将是新式类。对于新式类来说，在类D中调用对象value，它首先查找的依然是类D。  
如果没有查找到的话，他会依次查找B->C->A。在我看来，它是一种按层查找的原则，会在较低层查找对象，如果没有的话再到较高层查找。  
所以对于这个例子来说，value在类C中被查找到，所以它的值将会是3。  

# 参考资料
http://docs.python.org/2.7/
http://docs.python.org/3/

