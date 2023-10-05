---
layout: post
title: "jstack jmap resin"
date: 2012-02-01 20:41
comments: true
tags: jmap, jstack, resin
categories:
- knowledge 
---
Today, I use `jstack`, `jmap`, and `resin` to look up why my server is deading in stress test. And I also find printing information into log is useful. You can print log in entrance of function and exit of function.   
jstack is a tool which is looked up stack of system, specailly in threads. It is mainly for looking up dead lock and some functions which are executed too slowly.    
jmap is a tool which is looked up current memory of system or some memery jvm can not delete immediately.
You can use jstack like this:
```
jstack -l [pid]
```
If you know pid of the process, you can see what's the situation of stacks in process. If you want to see the detail, looking at those stacks.   
jmap is used like this:
```
jmap -heap [pid]
```
Today, Big niu Qiao tells me about JVM GC policy.   
There are New Generation, Old Generate and Perm Generation in JVM. You can find them in jmap command. And New Generation are new objects. And Old Generation are old objects which are still be refered. Eden Space stores new generate objects. And From Space stores still alive objects after gc once. To space stores objects which are not refered and in From space before gc once. If an objects is in From Space many times, it will be moved to Old Generation by JVM. Perm Generation are static objects and Classes.   
By the way, if your application runs in resin, you should look up those logs of resin, for example, jvm-default.log. The log file contains GC log information. If GC throws overMemory exception, you should modify your configuration of resin (resin.xml in resin4.0+). Modify or add:
```
<jvm-arg>-Xmx1024</jvm-arg>
``` 
This means set JVM heap space to 1G.
<!-- more -->  
今天使用了一下jstack, jmap来查看系统为啥在压力测试中缓慢的挂掉。我发现还是很有用的还有就是打log.函数入口和函数出口都打log。    
jstack是查看系统现在卡在那里，特别是各个线程在哪里，主要是排查死锁或者运行相当缓慢的函数。jmap是为了看系统的内存。当前使用的内存，以及jvm想删除却很久都没有删除的内存。    
jstack的使用方法如下
```
jstack -l [pid]
```
只要知道pid,就能知道该进程现在的stack情况。具体排查就是看那些stack...
    
jmap的使用基本一致
```
jmap -heap [pid]
```
今天继续听巧大牛讲JVM GC策略。
jmap中有New Generation 和Old Generation. 分别对应新生成的和很久都没有被释放的。Eden Space就是新生成的对象。而From Space是以前曾经存活的对象（一次GC之后，Eden区的对象都会转换为From区）。To Space是以前曾经存活的对象但是现在已经没有引用了（一次GC过后, Eden区全部对象和From区尚有引用的对象回全部放入到To区）。而对象如果一直在From区，就会被jvm放入到Old Generation中。
Old Generation是扫描一边标记，再扫一边回收，另外涉及到内存压缩云云。 
Perm Generation是static对象和class    
另外如果运行在resin中，还是看看resin的各个log.例如jvm-default.log.这个log中有关于GC的信息。如果GC已经OverMemery了.那就不用找了，直接在resin的conf中的resin.xml中找到jvm参数修改-Xmx的值即可。例如改为`-Xmx1024`即为java将使用内存1G.
