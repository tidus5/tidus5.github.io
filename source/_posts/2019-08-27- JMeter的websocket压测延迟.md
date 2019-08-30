title: JMeter的websocket压测延迟
date: 2019-08-27 19:34:47
categories:
- Java
tags:
- JMeter
---

最近在做WebSocket压测的时候，试用了apache的JMeter。
发现一个问题，压力不大的时候，也可能遇到延迟5秒的问题，在此做一下记录。

Jmeter默认不包含WebSocket的测试，需要单独下JMeter的插件。
首先到 https://jmeter-plugins.org/install/Install/ 下载JMeter的Plugin Manager。

按照网页上说的，把下载的jar 放到 JMeter 的lib/ext 目录下。重启JMeter。

在Option菜单，点开 Plugin Manager。 在Available Plugins中搜一下，可以看到有两个websocket插件。

**WebSocket Sampler by Maciej Zaleski**

**WebSocket Samplers by Peter Doornbosch**

开始，我选择安装的是第一个Maciej Zaleski的版本，去他的插件主页可以看到，有好几年没更新了。
其实这个插件运行有问题的，会出现压测延迟的问题。所以建议换另一个插件。下面可以不用看了。

 <!--more-->
 
JMeter可以切换中文的，在Option下 choose Language 即可选择。

JMeter基本测试，首先需要往Test Plan 下添加线程组。以下对线程组的参数大致做一下解释：

**线程数**：该线程组包括的线程数； 

**Ramp-up Period（in seconds）**：决定在多少秒内启动所有线程，即如果线程数设置为5，而此项设置也设置为5，那么会每隔5/5=1s启动一个线程； 

**循环次数**：即每个线程数要跑测试的次数。如果勾选永远，则会一直循环

（注意：如果勾选了永远且调度器配置中设置了持续时间，则会在持续时间到达之后结束循环）
 
 **Delay Thread creation until needed（延迟创建线程直到需要）**：对超级大线程数可以节约内存
 在JMeter 2.7以及更早的版本，测试一启动就会就会创建所有线程（分配内存），
 然后每个线程创建后根据延迟和Ramp-up来暂停执行自己的执行。
 那对于超级大的线程数，而且Ramp-up 也分配的很大的测试，就可能一开始创建所有线程就内存不足。
 
 比如，一万的线程数，Ramp-up设置一万秒，每个线程就跑1秒钟。其实永远只有一个线程在执行
 勾选后，每个线程到需要时才创建分配内存。避免开始就要分配一万条线程的内存而启动不了。
 
 调度器：勾选此项则打开调度器配置； 
 **持续时间（秒） **：即本线程组测试的持续时间，到时间后则停止此次测试，注意这个时间设置不要设置的比Ramp-up Period（in seconds）小，如果勾选了循环次数中的永远，那么测试一样会在此持续时间到达后结束； 
 **启动延迟（秒） **：此项设置为在我们启动测试后多久时间开始创建线程组，通常用于定时；
 
 设置好基本参数，比如10个线程，3秒内启动完毕，循环300次。总共发送3000条数据。
 
 然后，在线程组下，添加websocket Sampler 取样器。填入测试的IP，端口，发送信息。
 在线程组下添加监听器-> 聚合报告 和 查看结果树。
 然后点击启动，就可以开始测试了。
 
 这里遇到的问题，是这样10个线程，每个线程发送300条数据。经常会遇到有些消息发送延迟为5秒左右
 经过反复测试，都经常会遇到这个问题。
 
 最终试了半天，感觉服务器应该是没问题的。
 最终换了个 websocket 插件，就没有遇到这个问题了。
 
 所以，如果有遇到类似问题的，请使用 **WebSocket Samplers by Peter Doornbosch** 这个插件。


参考资料：
https://blog.csdn.net/df0128/article/details/80474252
http://jmeter.512774.n5.nabble.com/Delayed-thread-creation-was-OnDemand-ThreadGroup-td5714355.html