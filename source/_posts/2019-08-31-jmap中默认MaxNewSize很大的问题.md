title: jmap中默认MaxNewSize很大的问题
author: Tidus
date: 2019-08-31 00:00:40
categories:
tags:
---
用JDK7，在idea直接启动的时候，用jmap -heap pid 查看进程申请的内存的时候，发现一个问题。

···
Heap Configuration:
   MinHeapFreeRatio = 0
   MaxHeapFreeRatio = 100
   MaxHeapSize      = 4276092928 (4078.0MB)
   NewSize          = 1310720 (1.25MB)
   MaxNewSize       = 17592186044415 MB
   OldSize          = 5439488 (5.1875MB)
   NewRatio         = 2
   SurvivorRatio    = 8
   PermSize         = 21757952 (20.75MB)
   MaxPermSize      = 85983232 (82.0MB)
   G1HeapRegionSize = 0 (0.0MB)
···
这里MaxNewSize 是一个很大的数，而且格式也不大一样。那这个值是多少呢。

开始以为是显示错了，但除1024除了2次，得出1,677.72感觉数值也不大对。。。

后面搜到 http://www.voidcn.com/article/p-tgdkdxjc-bqp.html

再看了下，知道了是这样回事。

有个关键词Ergonomics。 如果jre 没有设置启动参数，则会用默认参数。 

同时，Ergonomics是java 自己会根据实际情况来决定实际采用的参数。

默认值的最大堆大小，是实际内存的1/4，而MaxNewSize，用的是uintx 字段的最大值。

在64位机上，就是2^64的字节，于是算下来就是 17592186044415 MB

然而，实际上，这时候交给Ergonomics 自主选择实际的新生代大小。 所以 jmap显示的，其实并不是最终实际选择的值。



参考：

http://www.voidcn.com/article/p-tgdkdxjc-bqp.html
https://www.iteye.com/blog/wangqiaowqo-2146356
https://gist.github.com/1363195 
https://stackoverflow.com/questions/22455562/what-set-the-value-of-jvm-parameter-maxnewsize-ergonomics
[jvm 参数默认值(jdk7)]https://my.oschina.net/u/2457218/blog/1544982
https://blog.csdn.net/ryo1060732496/article/details/85717553
https://blog.csdn.net/weixin_43194122/article/details/91526740