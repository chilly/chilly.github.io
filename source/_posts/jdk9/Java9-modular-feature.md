---
title: 'Java9:modular feature'
date: 2017-10-07 10:04:15
tags: ['java9','java','modular','module','model','feature','模块','功能','package','包']
categories:
- knowledge
- java
---

# 起源
这次写写JAVA9的modular，俗称模块化。这个应该是Java9的最出彩的地方。之前Java的那个项目叫做Jigsaw。 为什么会有这个项目呢？原因在于之前Java使用package作为管理的。大家为了图省事，里面写的class都是public class。 也就是说包外都可用。大家都使用各种包管理工具ivy, maven, gradle啥的，A依赖C的1.0版本, B依赖于C的2.0版本。然后A,B又被自己的工程所依赖。C的1.0版本和2.0版本中有相同的类，不同的函数定义。见下图：

<div align=center>
![jarhell.png](/images/jdk9/modular/modular_jar_hell.png)
**图 1** 运行时依赖不同版本的类
</div>

这就会导致JVM运行时不知道该用哪个类，这就是jar hell问题。不过jar hell还有许多种,这个只是比较常见的一种。

之前jdk自身的package也有许多，写着写着自己都搞不明白到底谁依赖谁了。然后大家就都需要依赖整个jdk的jar。但现在不一样了，只需要依赖一部分模块就行了。module是package的集合。而一个module可以依赖其他的module, 例如所有的module都依赖于java.base这个基础的module。

# 使用IDE，Intellij
说了这么多，不会用还是白搭。首先我们先用Intellij搭一个java9的工程。目录结构如下：

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


顺便一提，在IDE中Project上右键出来New...可以new出来新的module(自动带上了module-info.java)。 这个使用起来更加方便。我记得很久以前其实有package-info.java这个东西的。但是因为只是用来做文档声明的，大家嫌麻烦，都弃之不用了。我并不担心大家不用module-info.java. 因为这个东西还可以严格限制模块外可以调用哪些类。而且必须写模块依赖才行。所以如果你使用了module，那未来你一定有再次用到module的时候. 另外使用了module之后，你打成的最终jar应该更小巧。


仔细的研究一下module-info.java文件，里面有requires和exports。requires是依赖，exports是导出。这点倒是和JS很像。这两句任何一句缺失，你的程序编译时都会报错。错误类似：

```shell
只注释掉exports info.chillyc; 报错：
The module 'two' does not export the package 'info.chillyc' to module 'one'

只注释掉require two; 报错：
The module 'one' does not have the module 'two' in requirements

```


讲完了。你会用了吗？是不是很简单？....的确很简单。但是我觉得这依旧没有什么用。别人使用我的类的时候，如果我没有在module-info中exports出来，那他们要么重新写一个；要么让我改一下代码。


但是module的确让我们的工程代码更加清晰。不过怕就怕你把整个工程的package全部都exports出来，那这东西就没有什么用了。很多时候，我们封装好一个component,只希望暴露出少量的几个类。但是因为测试或者内部调用的方便，一般都会将class定义为public的。那这个时候使用module就可以很好的解决这个问题。


# 更实用的

对了，写点更加实用的。我们希望将各个module打成jar包，以后就可以单独使用某些module。这部分不能使用Intellij的IDE，因为它还没有支持。首先将Project的目录树变为如下的模样：

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
执行完之后就可以看到mods里面存放了编译好的class文件。继续执行打包命令,注意jar是jdk9里的才行：

```shell
$ mkdir mlib
$ jar --create --file=mlib/two@1.0.jar --module-version=1.0 -C modt/two .
$ jar --create --file=mlib/one@1.0.jar --module-version=1.0 --main-class=chillyc.info.Hello -C modt/one .
```

执行jar文件：

```shell
$ java -p mlib -m one
```

这个问题在于我每次执行就需要带上一个mlib的文件。这个文件很容易被被人直接拿去了反编译了。现在有更好的解决方案了：编译成一个image. 
```shell
$ jlink --module-path jmods:mlib --add-modules one --output oneapp
但是实际上上面这个要看你的Path是否正确，如果写成绝对路径应该像下面这样：
$ /Library/Java/JavaVirtualMachines/jdk-9.jdk/Contents/Home/bin/jlink --module-path /Library/Java/JavaVirtualMachines/jdk-9.jdk/Contents/Home/jmods:mlib --add-modules one --output oneapp
```

这样就有一个oneapp目录。这个目录里面你再也找不到你的one@1.0.jar这个东西了。如果你要执行，敲下面的命令
```shell
$ bin/java -m one
```
通过如下命令可以看一下bin/java这里的modules
```shell
$ bin/java --list-modules
结果:
java.base@9
one@1.0
two@1.0
```

java.base其实就是java9的默认模块，所有模块都会依赖java.base。上面打包成oneapp中的jmods就是java的基础模块。我觉得这个东西还是比较有用的。另外bin/java中里面暗含了这些模块，所以也可以直接执行刚才的jar。
```shell
$ bin/java -jar [你的相对路径]/one@1.0.jar 
```

# 结束

嗯。modular...其实也没有真正解决Jar Hell的问题。不过如果出现冲突，那么java9的提示会比较友好。另外没有一堆的classpath,这样ps命令时不会眼痛。编译成模块image这点对防止反编译还是有点用处的。但是这个image不能直接放到一个没有Java的环境中使用。那就是说，编译好的image, 仍然达不到开箱即用的效果。希望image这东西在Java10可以开箱即用，不再依赖OS和Jdk。(虽然官方文档说，编译出来的image文件可以放在没有JVM环境的地方使用。但是我Mac下编译出来的，为什不能在linux上使用呢？一点都不跨平台。这至少说明image依赖于OS...)



[Intellij的官方教程](https://blog.jetbrains.com/idea/2017/03/support-for-java-9-modules-in-intellij-idea-2017-1/)
[Jigsaw quick start](http://openjdk.java.net/projects/jigsaw/quick-start)
[modular开发手册 2016年版](http://openjdk.java.net/projects/jigsaw/talks/intro-modular-dev-j1-2016.pdf)
[modular images](http://openjdk.java.net/jeps/220)
