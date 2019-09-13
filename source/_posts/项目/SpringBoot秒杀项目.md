---
layout: article
title: SpringBoot秒杀项目(更新中……未完结)
date: 2019-05-08 00:00:00
updated: 2019-08-09 23:42:14
categories: 
  - 项目
  - 秒杀系统
tags: 
  - 项目

---

![âSpringbootâçå¾çæç´¢ç»æ](SpringBoot秒杀项目/spring-boot.png)

{% note success %}

本文为关于练习Java项目写下的相关笔记，涉及到SpringBoot、Tomcat云端部署、线程池优化、分布式Nginx、分布式token扩展、缓存系列Redis、本地缓存、lua缓存、cdn优化、令牌与RocketMQ消息队列实现流量削峰、JMeter压力测试等知识体系，因为是练习项目，笔记内容可能比较混杂，我会遇到问题不断的延伸，所以才有了这么一份总结

{% endnote %}

### 两种`archetype` `quickstart`和`webapp`的区别

`archetype`是骨架的意思，`maven`为我们开发制定好了一份`项目的原始模版`，我们可以在`原始模版`的`基础`上，进行`扩展`与`延伸`。

maven有提供各种各样的`模版`，这里只讲`quickstart`和`webapp`的区别

1. `maven-archetype-quickstart`
2. `maven-archetype-webapp`

<!-- more -->

`maven`配置路径如下，我用的是自带的`maven3`，仓库和配置文件的位置比较奇葩，请勿模仿，要放就放一个地方：

![image-20190909160722417](SpringBoot秒杀项目/image-20190909160722417.png)

##### 区别:项目结构不同

`quickstart`的项目结构图

![image-20190909160817443](SpringBoot秒杀项目/image-20190909160817443.png)

基本内容包括：

1. `一个包含junit的依赖生命的pom.xml`
2. `src/main/java主代码目录及一个名为App的类`
3. `src/test/java测试代码目录及一个名为AppTest的测试用例`

`webapp`的项目结构图

![img](SpringBoot秒杀项目/Center-20190909162327048.png)

基本内容包括：

1. `一个packaging为war且带有junit依赖声明的pom.xml`
2. `src/main/webapp/目录`
3. `src/main/webapp/index.jsp文件`
4. `src/main/webapp/WEB-INF/web.xml文件`

##### 为什么会有这样的结构的区别呢

其实两个骨架除了文件目录结构上的区别外，区别就是`quickstart`已经将`SpringBoot`的配置文件`application.properties`给创建好了，不需要我们额外的去创建该配置文件，是传统的`webapp`向`SpringBoot`的一个转型，为了`SpringBoot`去制定的一个`Archetype`

### 