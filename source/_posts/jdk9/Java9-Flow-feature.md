---
title: 'Java9:Flow feature'
date: 2017-09-29 21:00:04
tags: java9,java,jdk9,jdk,flow,Flow,feature,new
---

LP强烈要求用中文写。嗯，好吧，用中文写吧。

# Flow 简介
今天试用了一下jdk9, 最主要关注Flow这个新增的功能。结果发现，这个Flow其实是一个final class，构造函数还是private,没有任何正常方法将其实例化。这个是新功能？我抱着学习的心态看了一下java doc. 发现主要是几个interface. Interface...都是接口，到底有没有实现？

然后不知道怎么查到了reactive-stream, 其实Java的JVM Flow就是按照reactive-stream的API规范写的。

reactive-stream是什么？就是反向压力流（back pressure），其实你称其为反向控制流也行。官方说法是：一个异步非阻塞反向控制流处理的标准。嗯，相当拗口。

什么是反向控制？这里还是抄袭一下别人的图吧。图的出处在(这里)[https://community.oracle.com/docs/DOC-1006738]。

<div align=center>
![flow1.png](/images/jdk9/flow/flow1.png)
**图 1** 正常流处理
</div>

下面是反向的流处理：
<div align=center>
![flow2.png](/images/jdk9/flow/flow2.png)
**图 2** 反向流处理
</div>

看到了吧，反向的流**图 2**处理是Subcriber需要多少数据，就给多少数据。不要不给。正常的流**图 1**是管你要多少，我有多少给你多少。图1的问题就导致数据在管道里积压，或者在Subscriber里面积压。所以正常的流一般处理时要么数据就丢弃掉了，要么JVM就OOM了。所以反向的流好棒好棒的!

嗯，感觉也不是，因为有可能反向的流在Publisher的地方挤压了...其实你的程序性能不行，处理不完，积压不可避免。所以这反向的鬼东西有什么用呢？除非你的Publisher是根据你处理的数据的性能来生产数据。这听起来很酷，这很像当年教务系统的排队选课，以及魔兽世界中的等等等...其实就是处理性能不行，只能反推给最初的生产者**人**。当然还有一种能处理的情况是削峰，当大量数据涌入时会造成整个系统崩溃。但你的系统如果慢慢处理这些数据，还是完全可以处理好的。这样就增加了系统的稳定性。


Java9其实只是将这个标准写入了JVM中而已。我顺便看了一下Publisher以及Subscriber的实现类，很多都是incubate包的(即实验中)。只有一个类是特别的存在：SubmissionPublisher。


# Flow 使用
这个Flow使用真的好麻烦，最初我知道Flow这个Feature的时候，我以为只需要将数据push到流里，然后又一个default的Consumer获取到数据。这个Flow自己给我搞定了反向控制。No,No,No. Java9啥都没有做。啥都需要你自己写。如果没有SubmissionPublisher，那你需要根据那几个Interface自己去实现。这是什么鬼，我是不是可以宣布我写了一个可以实现全世界所有功能的框架呢？我的框架就是下面这样的：

```java
public interface Everything {

    void doEverything();
}
```
是不是非常棒?! Java9宣称的Flow就和我上面宣称的没有啥区别,只是它的接口更多而已。好了，它还是给出了一个SubmissionPublisher. 我就先原谅它吧... 可是怎么使用呢？我再次抄袭了[这边的blog的代码](https://community.oracle.com/docs/DOC-1006738)。我也不知道为啥变得这么厚颜无耻，不知脸红的抄袭。可能是跟腾讯阿里学的吧。

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
subscription.request(1)

```
# Flow 性能测试


# Flow bug
