---
layout: post
title:  "maven 安装jar"
date:   2013-12-18 09:36
categories: maven
---

1.[安装maven](http://maven.apache.org)  
2.设置环境变量
M2_HOME=/opt/maven/
PATH=$M2_HOME/bin:$PATH
验证：mvn --version
3.执行install.bat($project_home/lib/install.bat)