---
layout: article
title: JavaGuide阅读笔记
date: 2019-09-07 21:04:09
updated: 2019-09-09 21:04:09
categories: 
  - notFinished
tags: 
  - notFinished
---

{% note info %}

观开源博客项目`JavaGuide`，有些比较细枝末节的点，以及自己的延伸思考，想要重点记录一下

博客地址：https://snailclimb.gitee.io/javaguide/

{% endnote %}

### **Java 程序从源代码到运行一般有下面3步：**

![Javaç¨åºè¿è¡è¿ç¨](JavaGuide阅读笔记/Java 程序运行过程.png)

从`字节码`到`机器码`这一步，JVM类加载器会首先加载`字节码`文件，然后通过解释器逐行解释执行，称为边解释变执行

1. JIt和AOt
2. GPL
3. C++
4. JApplet
5. Char字节大小 不够放
6. 构造器不可以被重写
7. 