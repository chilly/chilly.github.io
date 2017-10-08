---
title: 'Java9:modular feature'
date: 2017-10-07 10:04:15
tags: ['java9','java','modular','module','model','feature','模块','功能','package','包']
---

# 起源
这次写写JAVA9的modular.俗称模块化。这个应该是Java9的最出彩的地方。之前Java的那个项目叫做Jigsaw. 为什么会有这个项目呢？原因在于之前Java使用package作为管理的。大家为了图省事，里面写的class都是public class. 也就是说包外都可用。大家都使用各种包管理工具ivy, maven, gradle啥的，A依赖C的1.0版本, B依赖于C的2.0版本。然后A,B又被自己的工程所依赖。C的1.0版本和2.0版本中有相同的类，不同的函数定义。见下图：

<div align=center>
![jarhell.png](/images/jdk9/modular/modular_jar_hell.png)
**图 1** 运行时依赖不同版本的类
</div>

这就会导致JVM运行时不知道该用哪个类，这就会导致jar hell的问题。不过jar hell还有许多种,这个只是比较常见的一种。

之前jdk自身的package也有许多，写着写着自己都搞不明白到底谁依赖谁了。然后大家都需要依赖整个jdk的jar。但现在不一样了，只需要依赖一部分模块就行了。module是package的集合。而一个module可以依赖其他的module, 例如所有的module都依赖于java.base这个基础的module.

# 使用IDE,Intellij
说了怎么多，不会用还是白搭。首先我们先用Intellij搭一个java9的工程。目录结构如下：

```
Project
    |
    module name
        |
        src
            |
            package name
                |
                class name
			|
			module-info.java
    |
    module name
        |
        src
            |
            package name
                |
                class name
			|
			module-info.java

```

这里有一个module-info.java。是写什么呢？首先我们将第一个module name写为one, 第二个module写为two. 第一个package是chillyc.info。第二个package name是info.chillyc.我们写一个最简单的Hello World代码，在代码里chillyc.info.Hello依赖于info.chillyc.World


Hello.java的代码
```java

package chillyc.info;


import static info.chillyc.World.WORLD;

public class Hello {
    public static void main(String[] args) {
        System.out.println("chillyc.info.Hello" + WORLD);
    }
}

```

World.java的代码
```java
package info.chillyc;

public class World {
    public static final String WORLD = "WORLD";
}

```

one的module-info.java
```java
module one {
    requires two;
}

```
two的module-info.java
```java
module two {
    exports info.chillyc;
}
```
呃，上面这些写成多个package，也不需要写module-info.java, 不是更省事吗？是是是，的确更省事，这些是为了整个java的生态环境。你一个人爽不叫爽，大家一起爽才行。

顺便一提，在IDE中Project上右键出来New...可以new出来新的module（自动带上了module-info.java）. 这个更加方便。我记得很久以前其实有package-info.java这个东西的。但是因为只是用来做文档声明的，大家嫌麻烦，都弃之不用了。我并不担心大家会不用module-info.java. 因为这个东西还可以限制模块外可以调用哪些类。而且必须增加模块依赖才行。所以如果你使用了module，那你未来在写代码的时候一定会再次用到module. 另外使用了module之后，你打成的最终包应该更小巧。

仔细的研究一下module-info.java文件，里面有requires 和 exports. requires是依赖，exports是导出。这点倒是和JS很像。这两句任何一句缺失，你的程序编译时都会报错。错误类似：
```
只注释掉exports info.chillyc; 报错：
The module 'two' does not export the package 'info.chillyc' to module 'one'

只注释掉require two; 报错：
The module 'one' does not have the module 'two' in requirements

```


讲完了。你会用了吗？是不是很简单。....的确很简单。但是我觉得这依旧没有什么用。就是别人使用我的类的时候，如果我没有在module-info中exports出来，那他们要么重新写一个。要么让我改一下代码。

但是module的确让我们的工程代码更加清晰。不过怕就怕你把整个工程的package全部都exports出来，那这东西就没有什么用了。很多时候，我们封装好一个component,只希望暴露出少量的几个类。但是因为测试或者内部调用的方便，一般都会将class定义为public的。那这个时候使用module就可以很好的解决这个问题。


# 更有用的部分

对了，写更有用的部分。我们希望将各个module打成jar包，这样想单独用某些module也是可以的。这部分不能使用Intellij的IDE。因为它还没有支持。首先将Project的目录树变为如下的模样：

```
Project
    |
    src
        |
        module name
            |
            package name
                |
                class name
            |
            module-info.java
    	|
    	module name
            |
            package name
                |
                class name
            |
            module-info.java
```

然后开始敲命令了。
```shell
首先确定你的javac, java, jlink什么的是不是java9的版本。
$ mkdir mods
$ javac -d mods --module-source-path src $(find src -name "*.java")

```
执行完之后就可以看到mods里面存放了编译好的class文件。继续执行打包命令,注意jar是jdk9里才行：
```shell
$ mkdir mlib
$ jar --create --file=mlib/two@1.0.jar --module-version=1.0 -C modt/two .
$ jar --create --file=mlib/one@1.0.jar --module-version=1.0 --main-class=chillyc.info.Hello -C modt/one .
```

执行jar文件：
```
java -p mlib -m one
```
