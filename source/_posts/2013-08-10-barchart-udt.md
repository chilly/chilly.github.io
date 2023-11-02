---
layout: post
title: "barchart UDT（一）"
date: 2013-08-10 03:27
comments: true
tags: jni,UDT,barchart,c++ 
categories:
- knowledge
- program
---

## UDT简单介绍

 &nbsp;&nbsp;&nbsp;&nbsp;最近在研究UDT相关的东西。这个主要用于高带宽时延积。也就是适用于网络带宽较高，但是丢包还是比较频繁的网络。给个公式：   

        高带宽时延积（缩写）=带宽 * RTT   

 &nbsp;&nbsp;&nbsp;&nbsp;UDT相关的可以在这里找到：[http://udt.sourceforge.net](http://udt.sourceforge.net) 另外对于UDT原版(c++)版，这里有两个下载路径。一个是CVS的。另外一个是git的。   

<!-- more -->

 * &nbsp;&nbsp;&nbsp;&nbsp;这个站点里，还有一个软件barchart UDT. 这个是jni的UDT.就是底层还是C/C++的库，上层wapper了Java.现在发布的最新的版本是2.3.0.第一个可以使用，而且大规模使用的版本是1.0.2.大家可以在网上找。这几次的升级加了很多没有用的东西。最核心的东西没有怎么变化。不过1.0.2没有javaDoc,基本上要先看一下UDT C版的源码才行。另外1.0.2现在已经没有了源码，因为开源的作者没有写清楚1.0.2是从那个version发布的（也可能是我看得粗糙）。   

 * &nbsp;&nbsp;&nbsp;&nbsp;这个站点里还有一款软件叫做UDT-JAVA.大家最好不要下，这个软件的网络行为和UDT原生的网络行为不太一致。另外这个软件在serverSocket.accept();这个地方开源的作者写的有bug。我使用一次UDT连接时，竟然可以获取到多个UDT socket.不管是怎么回事，最后还是弃之不用了。   

## barchart UDT  (mvn需要3.0.4以上版本)

这里要着重介绍这个UDT.最好使用的版本是2.3.0。不过该版本从git上download下来。编译花了我一天的功夫。第一个问题就是贵司集群上该死的mvn环境。这个环境是默认的repository是在~/.m2/repository. 这个环境会报：`java.io.IOException: No locks available`.这个问题就是需要你在\$M2\_home/conf/settings.xml中加入：`<localRepository>/your-path/.m2/repository</localRepository>` 这里的路径需要是绝对路径。第二个问题，贵司没有\$M2\_home这个变量!这个变量切不可自己随便写，必须找到那个安装的路径,否则mvn不能被执行。当然贵司是不可能有人知道这个路径的。所以...解决方法就是使用`mvn -s your-conf`来执行mvn.   

下面看起来就可以直接执行mvn install了。作者说最好进入barchart-udt-archon再执行. 然后遇到这个错误：`Exception in thread "main" java.lang.NoSuchMethodError: org.apache.maven.wagon.shared.http4.AbstractHttpClientWagon.getReadTimeout()I` 这个错误是mvn的，不过更加可气。这个解决方法就是在pom.xml中加入：

        <extensions>
          <!-- Whysoever version 2.2 has issues with Maven 3.0.3:
               java.lang.NoSuchMethodError: org.apache.maven.wagon.shared.http4.AbstractHttpClientWagon.getReadTimeout()I
          -->
          <extension>
            <groupId>org.apache.maven.wagon</groupId>
            <artifactId>wagon-ssh</artifactId>
            <version>2.1</version>
          </extension>
          <extension>
            <groupId>org.apache.maven.wagon</groupId>
            <artifactId>wagon-http</artifactId>
            <version>2.1</version>
          </extension>
          <extension>
            <groupId>org.apache.maven.wagon</groupId>
            <artifactId>wagon-file</artifactId>
            <version>2.1</version>
          </extension>
        </extensions>   

上面这块代码必须放在 `<project><build>放在这里</build></project>`, 否则mvn会报no Tags错误。  

看起来可以正常运行了。结果......更加悲剧的事情是mvn需要3.0.4版本。只好在官网上download一个高版本的maven.然后在集群上解压。执行ant命令。这里需要首先设置\$M2\_HOME这个变量。然后看官网的文档安装。不过这还不算完，最后编译barchart-udt后，bundle那个项目一直不能正常的编译。不过如果你只是为了替换某些不能运行的libxxx.so文件，只要编译完了core项目。就可以从core项目中找到相应的.so文件。然后下载一个2.3.0的jar包。解压后，把相应的文件替换掉。然后再打成jar包。打jar包命令：`jar c [jarfile] -m [MANIFEST file]  [other files]`    

然后放在你的程序中运行即可。barchart-udt的那个Jar可以自动解压出来so文件，然后放在相应的地方。之后就可以使用jni调用了。和正常的使用java socket一样。下一篇会讲如何使用barchart-udt进行编程。
