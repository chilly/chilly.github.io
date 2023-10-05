---
title: 'Java9:Flow feature'
date: 2017-09-29 21:00:04
tags: ['java9','java','jdk9','jdk','Flow','feature','new','Publisher','Subscriber','Processor','performance']
categories:
- java
---

LP强烈要求用中文写。嗯，好吧，用中文写吧。

# Flow 简介
今天试用了一下jdk9, 最主要关注Flow这个新增的功能。结果发现，这个Flow其实是一个final class，构造函数还是private,没有任何正常方法将其实例化。这个是新功能？我抱着学习的心态看了一下java doc. 发现主要是几个interface. Interface...都是接口，到底有没有实现？

然后不知道怎么查到了reactive-stream, 其实Java的JVM Flow就是按照reactive-stream的API规范写的。

reactive-stream是什么？就是反向压力流（back pressure），其实你称其为反向控制流也行。官方说法是：一个异步非阻塞反向控制流处理的标准。嗯，相当拗口。

什么是反向控制？这里还是抄袭一下别人的图吧。图的出处在[这里](https://community.oracle.com/docs/DOC-1006738)。

<div align=center>
![flow1.png](/images/jdk9/flow/flow1.png)
**图 1** 正常流处理
</div>

下面是反向的流处理：
<div align=center>
![flow2.png](/images/jdk9/flow/flow2.png)
**图 2** 反向流处理
</div>

看到了吧，反向的流处理**图 2**是Subcriber需要多少数据，就给多少数据。不要不给。正常的流处理**图 1**是管你要多少，我有多少给你多少。图1的问题就导致数据在管道里积压，或者在Subscriber里面积压。所以正常的流一般处理时要么数据就丢弃掉了，要么JVM就OOM了。所以反向的流好棒好棒的!

嗯，感觉也不是，因为有可能反向的流在Publisher的地方挤压了...其实你的程序性能不行，处理不完，积压不可避免。所以这反向的鬼东西有什么用呢？除非你的Publisher是根据你处理的数据的性能来生产数据。这听起来很酷，这很像当年教务系统的排队选课，以及魔兽世界中的等等等...其实就是处理性能不行，只能反推给最初的生产者**人**。当然还有一种使用场景是消除峰值，当大量数据瞬间涌入时会造成整个系统崩溃。但如果你的系统在限定的时间内慢慢处理这些数据，还是完全没有问题的。这样就增加了系统的稳定性。


Java9其实只是将这个标准写入了JVM中而已。我顺便看了一下Publisher以及Subscriber的实现类，很多都是incubate包的(即实验中)。只有一个类是特别的存在：SubmissionPublisher。


# Flow 使用
这个Flow使用起来，真的好麻烦。最初我知道Flow这个Feature的时候，我以为只需要将数据push到流里，然后有一个default的Consumer获取到数据。这个Flow自己给我搞定了反向控制。No,No,No. Java9啥都没有做。啥都需要你自己写。如果没有SubmissionPublisher，那你需要根据那几个Interface自己去实现。这是什么鬼，我是不是可以宣布我写了一个可以实现全世界所有功能的框架呢？我的框架就是下面这样的：

```java
public interface Everything {

    void doEverything();
}
```
是不是非常棒?! Java9宣称的Flow就和我上面宣称的没有啥区别,只是它的接口更多而已。好了，它还是给出了一个SubmissionPublisher。 我就先原谅它吧... 可是怎么使用呢？我再次抄袭了[这边的blog的代码](https://community.oracle.com/docs/DOC-1006738)。我也不知道为啥变得这么厚颜无耻，不知脸红的抄袭，可能是跟腾讯阿里学的吧。

## Publisher和Subscriber简单示例

```java
public static class MySubscriber<T> implements Flow.Subscriber<T> {
        private Flow.Subscription subscription;

        @Override
        public void onSubscribe(Flow.Subscription subscription) {
            this.subscription = subscription;
            subscription.request(1); //这里要使用Long.MAX_VALUE就会被认为获取无穷的数据。
        }

        @Override
        public void onNext(T item) {
            System.out.println("Got : " + item);
            subscription.request(1); //这里也可以使用Long.MAX_VALUE
        }

        @Override
        public void onError(Throwable t) {
            t.printStackTrace();
        }

        @Override
        public void onComplete() {
            System.out.println("Done");
        }
    }
```

```java
public static void main(String[] args) throws InterruptedException {
        SubmissionPublisher<String> publisher = new SubmissionPublisher<>();

        //Register Subscriber
        MySubscriber<String> subscriber = new MySubscriber<>();
        publisher.subscribe(subscriber);

        //Publish items
        System.out.println("Publishing Items...");
        String[] items = {"1", "x", "2", "x", "3", "x"};
        Arrays.asList(items).stream().forEach(i -> publisher.submit(i));
        publisher.close();
        Thread.sleep(20000);
    }
```
主要的函数是：
```java
// 反向控制获取数据个数。
//代码里两处request(1)都不能丢，否则数据无法正常获取
subscription.request(1)
// 发布数据，相当于数据输入
publisher.submit(i)
// 关闭publisher，没有该函数则Subscriber.onComplete()不会被调用。
publisher.close()  
// 因为是异步的流处理
// 所以没能提供同步的接口，可以自己在Subcriber中加入同步策略
// 我这里简化了,如果你去掉这段代码，那你可能什么都看不到
Thread.sleep(20000);
```

运行结果如下：
```shell
Publishing Items...
Got : 1
Got : x
Got : 2
Got : x
Got : 3
Got : x
Done
```

## Flow的Processor接口

Flow还有一个重要的Processor接口，这个其实就是函数变换。其实一般的函数变换操作都可以在Subscriber中实现，Processor并没有什么卵用。不过如果让我去定义接口，我仍然会给出Processor. 为啥？这样会防止别人来喷我...嗯，其实给出了Processor接口，一样有人来喷，例如我...

看下图就知道我在说什么了。竟然无图可盗了...

<div align=center>
![withProcessor.png](/images/jdk9/flow/withProcessor.png)
**图 3** 带有Processor的流
</div>

<div align=center>
![withoutProcessor.png](/images/jdk9/flow/withoutProcessor.png)
**图 4** 没有Processor的流
</div>

如果增加另外一个流，就可以完全实现Processor的功能。所以表达流的**最小原语**不应该存在Processor这东西。但是Reactive-Stream说：我的这个就是规范。那好吧，就加一个吧。不过增加Processor的好处在于：你可以只写一个流，里面有很多Processor。而不是写很多流，将Subscriber和Publisher混在一起写。虽然你已经在Processor中将Publisher,Subscriber混在一起写了...嗯...呃...总而言之，你一定要相信规范，Processor存在一定有它的道理。

下面也给一个Process的例子：是过滤用的Processor.(这段代码是我自己写的，在Oracle官方文档中本应存在这段代码，但是他们不小心忘记写了...忘记写了...我看到的官方文档最后由 Bob Rhubart-Oracle 于 2016-9-26 下午2:03修改,版本号为6)：
```java

public static class MyFilterProcessor<T,K extends T> extends SubmissionPublisher<K> implements Flow
            .Processor<T, K> {

        private Function<? super T, Boolean> function;
        private Flow.Subscription subscription;

        public MyFilterProcessor(Function<? super T, Boolean> function) {
            super();
            this.function = function;
        }

        @Override
        public void onSubscribe(Flow.Subscription subscription) {
            this.subscription = subscription;
            subscription.request(1);
        }

        @Override
        public void onNext(T item) {
            if (!(boolean) function.apply(item)) {

                submit((K)item);
            }
            subscription.request(1);
        }

        @Override
        public void onError(Throwable t) {
            t.printStackTrace();
        }

        @Override
        public void onComplete() {
            close();
        }
    }
```

```java
public static void main(String[] args) throws InterruptedException {
        //Create Publisher
        SubmissionPublisher<String> publisher = new SubmissionPublisher<>();

        //Create Processor and Subscriber
        MyFilterProcessor<String, String> filterProcessor = new MyFilterProcessor<>(s -> s.equals("x"));

        MySubscriber<Integer> subscriber = new MySubscriber<>();

        //Chain Processor and Subscriber
        publisher.subscribe(filterProcessor);
        filterProcessor.subscribe(subscriber);

        System.out.println("Publishing Items...");

        String[] items = {"1", "x", "2", "x", "3", "x"};
        Arrays.asList(items).stream().forEach(i -> publisher.submit(i));
        publisher.close();
        Thread.sleep(2000);
    }
```

运行结果如下：

```shell
Publishing Items...
Got : 1
Got : 2
Got : 3
Done
```
重要的代码片段是：
```java
if (!(boolean) function.apply(item)) {
	submit((K)item);
}
```
这里的submit就是数据再发布，相信大家都能看懂，不再叙述。


# Flow 性能测试

到底这个东西性能怎么样？我是不是随便写一个Flow的实现都比它快？如果它性能差，我按它这个规范写有什么意义？为了好看吗？
好吧。我自己用无锁队列实现了一下。功能就是简单的计数，看看各自的最强性能。

```shell
无锁队列执行结果：
class chillyc.info.speed.old.XFlow:27qps
class chillyc.info.speed.old.XFlow:171629qps
class chillyc.info.speed.old.XFlow:7586111qps
class chillyc.info.speed.old.XFlow:8095748qps
class chillyc.info.speed.old.XFlow:7640866qps
class chillyc.info.speed.old.XFlow:6845523qps
class chillyc.info.speed.old.XFlow:6516655qps
class chillyc.info.speed.old.XFlow:6191659qps
class chillyc.info.speed.old.XFlow:5742711qps
class chillyc.info.speed.old.XFlow:6657964qps
class chillyc.info.speed.old.XFlow:5720992qps
```
JVM jdk9 Flow执行结果
```shell
Concurrent.Flow 测试结果： 设置无穷索取, request(Long.MAX_VALUE)
class chillyc.info.speed.jdk9flow.Jdk9Flow:39688qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:6798618qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:6556238qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:6506791qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:6545895qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:7129085qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:7005827qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:6818252qps
```

性能对比图：
<div align=center>
![性能对比](/images/jdk9/flow/speed.png)
**图 5** 性能对比图
</div>

个人感觉前两次都可以忽略不计，无锁队列的实现中，前两次线程还没有完全启动，数据还没有完全填充。不过从另外一个侧面反映出jdk9 Flow启动效率非常高。我们除去前两次计算一下平均值。看到两者相差不大:无锁队列比jdk9 Flow平均快了17000+qps.

实验和数据量，buffer大小都有关系，变量太多，没有一一实验，并且两者的qps都是六七百万量级，所以我认为两者差别不大。JDK9的Flow实现的挺不错的，竟然能和我顺手写的性能差不多。我心中不由的赞叹一番。

# Flow bug
因为测试了jdk9 Flow的无限索取时的性能。我想再测测每次取一个导致的最差性能（request(1)）。结果就发现了bug.
测试输出如下：
```shell
class chillyc.info.speed.jdk9flow.Jdk9Flow:55296qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:0qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:0qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:0qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:0qps
class chillyc.info.speed.jdk9flow.Jdk9Flow:0qps
```
从结果上分析，经过一段时间之后，jdk9的Flow就无法处理数据了。这...真的能用吗？


# 结束语
本来应该带着虔诚敬畏的心态学习JDK9的...结果变成了吐槽大会了...罪过罪过，实在不该这样...顺便一提，jshell非常好用，tab功能提示很强大，就是我机器性能有点差，感觉很卡顿... 

对了，不要以为我只会讨论jdk9...那是因为ReactJS 16发布的比较晚。
