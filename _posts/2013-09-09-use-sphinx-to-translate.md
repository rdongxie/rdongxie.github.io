---
layout: post
title:  "使用Sphinx(python)进行翻译"
date:   2013-09-09 22:06:57
categories: translation
---

### 1.使用reStructuredText,进行项目文档编写

reStructuredText是文本标记语言--和markdown相似,现在很多项目使用它来编写文档  
Sublime Text2官方网站给出的第三方文档：https://github.com/guillermooo/sublime-undocs ，  
reStructuredText是python的工具包docutils为文档编写设计的标记语言。  
reStructuredText官方网站： http://docutils.sourceforge.net/rst.html  
维基百科：http://zh.wikipedia.org/zh-cn/ReStructuredText‎  
其他网站：http://wstudio.web.fc2.com/others/restructuredtext.html‎  
http://sphinx-doc-zh.readthedocs.org/en/latest/rest.html

### 2.关于GNU gettext

Gettext 用于系统的国际化(I18N)和本地化(L10N),可以在编译程序的时候使用本国语言支持(Native Language Support(NLS)),  
其可以使程序的输出使用用户设置。Windows: http://gnuwin32.sourceforge.net/packages/gettext.htm  
官方网站：http://www.gnu.org/software/gettext/  
tips:gettext中支持的语言包括RST，这个RST并非reStructuredText标记语言，指的是Resource String Table,


### 3.Sphinx

这个Sphinx并不是搜索引擎，而是文档产生工具。  
官方网站：http://sphinx-doc.org  
国际化：http://sphinx-doc.org/intl.html  

使用Sphinx步骤：  
源文件(*.rst)--(Sphinx)-(xgettext)-->*.pot---(poedit)-->*.po------(msgfmt)---(poedit)-->*.mo。  
*.pot,*.po给人阅读，*.mo给机器在国际化处理时候使用  
0.安装Sphinx  
pip install Sphinx 或者：easy_install -U Sphinx 

1.使用sphinx-quickstart去创建一个空的文档项目(产出一些符合Sphinx要求的目录结构和文件例如conf.py)  ,如果是在别人建好的项目上进行翻译，则省略这一步。  
	sphinx-quickstart

2.在Sphinx产生的source文件夹中使用reStructuredText进行文档编写，如果在别人编写好的文档上进行翻译，则省略这一步。

3.*.rst--->*.pot  
这个过程使用Sphinx进行转换，貌似也可以使用xgettext进行转换(i dont know how to do),  
	sphinx-build -b gettext ./source ./pots

4.*.pot--->*.po  
这个过程是翻译人员填充翻译内容，可以使用专用的翻译工具poedit，或者使用编辑器进行编辑，注意填充头信息

5.*.po--->*.mo  
使用msgfmt进行编译，使用poedit进行翻译，会自动生成*.mo文件

6.修改配置文件conf.py  
在Sphinx中增加，locale_dirs = ['../translations']  
其中'../translations'为存放*.mo文件的文件夹，相对source(或者是conf.py)的目录，  
在translations中的文件结构为：..\translations\zh_CN\LC_MESSAGES\ *.mo

7.生成对应语言的文档
需要使用-D选项设置语言，例如生成html文档：  
	sphinx-build -Dlanguage=zh_CN -b html ./source ./zhCNhtml  

### 3todolist:

1.rst文件变更后生成pot，po的merge

2.细节修正

3.....