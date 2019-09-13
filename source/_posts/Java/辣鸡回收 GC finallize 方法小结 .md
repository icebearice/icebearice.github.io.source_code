---
layout: article
title: 垃圾回收GC finalize方法 小结
date: 2019-08-29 22:20:40
updated: 2019-08-29 22:20:40
categories: 
  - Java
tags: 
  - Java
---

## 垃圾回收GC finalize方法 小结

> [来源]: https://medium.com/@clu1022/java筆記-final-finally-與-finalize-d72dc66e49eb
>
> 這是屬於 java.lang.Object 上的一個 method, 其設計目的是保證物件在被 GC 之前完成特定資源的回收作業. 不過由於這東西真的太不穩定了, 所以從 JDK 9 開始已經被標成 deprecated 了.
>
> 至於這個…, 就把它當作時代的眼淚吧. 這在業界裡基本上是一再證明不是個好方案了, 所以在 JDK 9 中已經被標註為 deprecated 了. 除非你有特殊原因, 不然不要去實作這個方法, 也不要期待透過這個方法來進行 GC. 原因很簡單: 因為你沒有辦法確保 finalize 的執行時間. 就算執行了, 也不確定是否能符合預期 (這要扯到 JVM 的 GC 機制, 這邊沒地方寫了). 使用不當還會影響性能, 進而導致 dead lock, hanging 等問題.

#### 手动垃圾回收的三种方式

- 其实小标题是一种误导，这根本不是三种方式，因为我们可以做的只是通过手动的形式 增加一些机会 执行一些内存清理释放的方法 的机会
  1. 将对象设为null，GC会优先回收 String xx = null;
  2. finallize方法，GC在回收对象之前调用该方法，你可以重写此方法来增加一些close和什么鬼的方法，统一资源清理（已经不推荐了）
  3. System.gc 会添加GC的机会，其实这个gc()函数的作用只是提醒虚拟机：程序员希望进行一次垃圾回收。但是它不能保证垃圾回收一定会进行，而且具体什么时候进行是取决于具体的虚拟机的，不同的虚拟机有不同的对策。
<!--more-->
- finalize()是Object的protected方法，子类可以覆盖该方法以实现资源清理工作，GC在回收对象之前调用该方法。
  - finalize()与C++中的析构函数不是对应的。C++中的析构函数调用的时机是确定的（对象离开作用域或delete掉），但Java中的finalize的调用具有不确定性
  - 不建议用finalize方法完成“非内存资源”的清理工作，但建议用于：① 清理本地对象(通过JNI创建的对象)；② 作为确保某些非内存资源(如Socket、文件等)释放的一个补充：在finalize方法中显式调用其他资源释放方法。其原因可见下文[finalize的问题]
- System.gc()与System.runFinalization()方法增加了finalize方法执行的机会，但不可盲目依赖它们

这东西 有点难讲清楚 我决定等我复习 虚拟机那本书 再来看看吧 

[参考链接]: https://www.jianshu.com/p/e1b2db7bafce



