---
title: Code Review工程实践
date: 2017-07-27 21:07:24
tags: ['code','review','工程实践','好处','推动']
categories:
- [project]
---

本文分三个部分：先讲Review代码的流程，再讲Review的技巧，以及如何推动公司层面Code Review. 

1. Review流程这一部分比较简短。流程多变,但都是为了发现代码的问题，反复建议提交者修改。
1. Review技巧我这部分分两个章节介绍。一章主要讲如何阅读代码，reivew的重点在于阅读。与读文章一样，也分粗读和细读。另外一章讲review代码的重点考察点，比较散乱也比较长。如何评论别人的代码并不是本文的内容。下面的言论也不是评论别人代码时用到的，所以请大家注意一下。以后会写一篇文章详细阐述如何评论别人的代码。
1. 推动公司进行Code Review， 这一章节纯属于个人的愚见，大家有更好的想法也欢迎讨论。

# 第一章 Review流程
<div align=center>

![reviewflow](/images/code-review/reviewstate.png)

</div>
写完代码后，发送diff给指定人review。待给出意见后，修改代码，继续提交diff，直到review通过。通过review的代码由审阅者自动或者手工Merge代码到指定分支。多人协作review时，第一位阅读者给出粗略意见，第二位看意见是否给的恰当，再补充其他意见。多人同时review,也可将代码用投影仪打到大屏，共同review一段代码。

这里需要思考几个问题：

1. review的分工。有多人参与时需要考虑这个问题。最好有一个总负责人。
1. review的策略。项目初期应该主要考察代码结构是否符合规范，即代码是否放到了相应的位置。项目中期应该以逻辑正确、简化代码、重构现有代码为主要考察点。项目后期主要看程序性能相关的优化问题。
1. review通过的策略。 什么样的代码才能通过review。 是不是需要完全符合checklist? 我个人并不赞同完全遵照checklist。后面我会细说。
1. 如何反馈已修改？review代码的工具一般是gitlab, reviewboard, gerrit。这些工具都提供了comments这一功能。review的时候可以写下comments。 如果对照comments进行修改，那么在comments后回复已经修改即可。
1. 如何反馈拒绝？这个最好找审阅者当面说清楚:为什么不按照建议编写代码。如果认为自己文笔非凡的也可以在review工具中写明白。
1. review修改后再次review的策略。要对照之前的comments看看编码者是如何修改的。当然也要看看有什么新加入的代码。
1. 代码没有必要写完后再review。可以写一部分review一部分。

# 第二章 阅读代码的技巧
<div align=center>

![代码](/images/code-review/code.png)

</div>
如何阅读别人提交的代码？首先要了解这代码的上下文和需求，不能不负责的指点江山。要充分了解需求，才能考虑如何实现，然后才能审阅别人的代码。不能因为读不懂别人的代码就拒绝别人的代码。这事开源界常发生，但公司内部最好杜绝发生。大家都是同事，低头不见抬头见，没必要关系僵化。你是能炒别人鱿鱼还是你自己要离职呢？看不懂代码可能有三种：一种使用了混淆、一种提交者写的烂、一种审查者能力不足。

混淆代码一般都是有特殊原因的，但不会是故意刁难审阅者。比如说机密的模块、需要隐藏秘钥或算法。这部分代码讲明白意思即可，一般也不需要review。

如果提交者写的太烂，一般都是i,j,k的命名导致。审阅者更应该读懂后，指出哪些地方需要改进：比如少写了注释，改个好名字等。写的烂并不会导致你读不懂，一般是你不愿意花时间去读。如果你说事不关己，没必要看烂的代码。这种心态也不适合进行Code Review，只适合小作坊编程。不如谦虚谨慎、虚怀若谷。

最后一个原因：请先提高自身的编程能力！有许多算法精妙难懂，在自己编程水平不足的时候决不能以读不懂为由拒掉。

当你有足够的编程能力时，review代码时请遵照下面的流程：

```
粗读（知道大概写了哪些内容） 
=> 抽取重点（架构/类的定义/代码结构）
=> 细读代码（简化/重构）
=> 逻辑判断 (各分支)
=> 细节优化（资源释放，线程安全、算法效率）
```

1. 通过粗读，我们需知提交者改动了什么。是大量修改？还是大量新增？有没有针对性的测试？没有改动配置文件? 有没有改动数据库？
1. 我们再仔细看看设计是否恰好符合了需求。是顺序的代码，用到了多线程？还是使用了设计模式？抑或用了第三方组件/架构？是集中式，还是分布式？数据是怎么存储的？单元测试有没有测重点？还有没有更好的设计？
1. 再细读一遍代码。挑出繁琐的代码与相似的代码。看是否有简化或重构的可能。嵌套是否太深。是否有难懂的代码。
1. 仔细寻找逻辑判读的语句if，while，for等。主要看每一个分支是否符合需求，是否是笔误。
1. 最后把你的注意力集中在资源释放、线程安全、算法效率上。看看有没有优化的空间。


## 第三章 代码考察点

如果你使用IDE，例如IDEA. 在IDEA中点击Analyze =&gt; Inspect Code ... 就可以做静态检查。主要检查拼写、静态变量、包和类名等. 提交者应该用这些工具在提交代码前自查，修改一些简单的语法优化和单词拼写错误，然后再交给别人审核。


编写代码如同写文章一样；审阅代码和审阅文章也有相通之处。这里大部分技巧来自审阅文章的方法。我这里不讲Java语法的东西，也不讲性能优化的事情。那些代码太特殊，太依赖于特定的语言和上下文。我这里将以Java代码为例，但你也可以将这些技巧用于审阅其他的语言。我审阅代码时一般考察以下几点：

### 3.1 优雅即正义
什么是优雅的代码？我觉得整齐划一的代码不一定是优雅的代码。python的代码都是优雅的？其实不管什么语言，总会写出烂代码。简练的代码不一定是优雅的代码。优雅的代码不仅格式让人舒服，而且简练易读。读过之后，清楚代码到底做了什么事，了解它在运行时大概是什么样子的，知道在哪里改动代码来满足新的需求。

如果你是一个熟知数据结构、算法、设计模式、成熟的架构、编程语法、常用中间件、操作系统的程序员，通读了代码，却无法回答以上问题，那就不是优雅的代码。一定是哪里写的比较混乱，或者隐藏了一些时序因果。不是优雅的代码，就一定有需要改动的地方。

### 3.2 姓谁名何
名字很重要。你所知道的各大公司。Google是什么？是一个名字。阿里巴巴是什么？也是一个名字。你能数出来的各大品牌，都是一个个名字,所以名字很重要。名字能让你区分事物。你不应该把所有的变量都命名为a,b,c,i,j,k.各位的家长也没有给大家取名叫你，我，他。所以当你review类名/变量名/函数名的时候，请想想能否起一个更好记更易懂的名字。


### 3.3 份内之事
一个类/函数能做完的事情，不要在类/函数外完成。下面是一个执行远程cmd并返回结果的代码片段。
```
ResultEntity result = CommandUtil.exeCommandUtil(user, location, cmd, host, opt); 
result.setSubmitTime(time); 

```
为什么result的提交时间不能在exeCommandUtil里面补全？这函数如果出现了十几、二十几处。那每一处都要多写一行代码。能清楚的分辨类和函数的职责并不是一件容易的事情，需要大量的经验积累。其实我们大多数人连本职工作是什么都不知道，就更难写出知道自己要干什么的类/函数了。现实中我们经常遇到本应该那人份内之事，你却要催促他完成。本应该他将工作进度告知你，你却要不断询问他完成的怎么样了。本应该他代码处理的事情，非要你帮他二次处理一下。


### 3.4 看了代码，开始怀疑人生
很多时候看代码看不懂，或者很迷茫。说白了，是审阅者和编码者脑回路不匹配导致。主要是因为编码者实现了太多的子类，或者类的层次太深，或者子类之间没有什么共同点。有时候，编码者用消息中间件实现了组件之间的分离，代码中隐藏了生产者与消费者之间联系。我们人类是按照时序思考的，任何破坏时序因果的代码都会让我们感觉迷茫。但这样的代码并不代表质量不高。只是希望改动后能更容易读懂。对于子类中没有什么共同点的可能需要拆开重构；对于消息分离的需要多加些注释，在注释中告诉读者下一阶段的处理流程是什么。如果是生产者，需要在注释中告诉读者，生产的某一消息，将被哪些消费者使用。如果是消费者，可以在注释中说明一下可能获取怎样的消息。


### 3.5 逸马毙犬于道
有些人写代码总免不了说废话。精炼自己/别人的代码。代码越精简，开发/阅读速度就越快。例如：
```java
// This function is for adding user
public void addUser() {
    
}

```

我觉得上面的开发者绞尽脑汁才写出了那一行注释。再如：
```java
if (a > b) {
    return a;
} else {
    return b;
}
```

### 3.6 言不达意

需求是增加加入组织、退出组织的接口。实现者为了开发方便，将功能放入到了一个handler里面，然后用一个enum Type来标示操作类型。而Type使用了位与操作来判断可能的类型。
```java
if (JOIN.isContainType(type)) {
	try {
		join(group);
	} catch (Exception e) {
		LOG.warn("join group with wrong.params:" + JSON.toJSONString(params), e);
	}
}
if (EXIT.isContainType(type)) {
    // do exit.
}
```

type可能是多个操作的集合吗？那*JOIN*这个单一操作怎么可能包含多操作type？名字也有问题。是不是改成下面的代码更好？

```java
if (op.isContainOp(JOIN)) {
	try {
		join(group);
	} catch (Exception e) {
		LOG.warn("join group with wrong.params:" + JSON.toJSONString(params), e);
	}
}
if (op.isContainOp(EXIT)) {
     // do exit group.
}
```

再想想，其实op只可能是单一操作。看上下文请求这一个接口不可能既加入组织又退出组织。这实在太混乱了。万一不小心将EXIT判断写到JOIN前面，岂不是先退出组织再加入组织？所以合理的代码只能是下面这种

```java
switch(op) {
    case JOIN:
        // do join group
        break;
    case EXIT:
        // do exit group
        break;
}
```

### 3.7 0/1日志,图灵在世
打印log的代码和下面两句非常类似：一条是正确的日志，一条是错误的日志。
```java
log.info("user is login");
log.warn("user op error.");
```

输出日志可能是这样的：

```bash
user is login
user is login
user is login
user op error.
user op error.
user is login
user is login
```

What the FFFFFFFF...... 我看日志我怎么知道是谁login了？又是谁做了什么导致错误了？那与只打印0, 1又有什么区别？例如：

```bash
1
1
1
0
0
1
1
```
以后都改成这个就好了。还省代码和时间。甚至连脑子都省了。

### 3.8 那不是一个人在战斗
思维要保持一惯性,比如函数签名。如果某个变量一直出现在另外一个变量之前，那么请保持变量的相对位置不变。另外变量名也要尽可能的一致。审阅这些代码的时候，我总有一种错觉：那不是一个人写的代码，一定有人在帮他写代码。
```java
public void copy(String srcPath, String dstPath);
public void mv(String srcPath, String dstPath);
public void del(String path);
public void innerCopy(boolean force, String dest, String src);
```

src都应该在dst前面。取名最好都是srcPath和dstPath，innerCopy最好这样写：

```java
public void innerCopy(String srcPath,String dstPath, boolean force);
```


### 3.9 不做标题党
```java
public String getScript(String appId, JSONArray jobSysParams, JSONArray appParams, String userId) {
	...
	...
	ExecShellUtils.execShell(script);
	return Path.join(...);
}
```
起初我看到这个函数名时，我觉得这应该是一个获取用户在某个应用下的脚本。当看到ExecShellUtils.execShell，我觉得这函数应该是直接执行了这个脚本。然后又看到return Path.join()，这怎么又返回了这个脚本的路径？这样的话，函数名应该叫execScriptAndReturnScriptPath()。


### 3.10 请站在巨人的肩上

多使用jdk,自有库和被验证的第三方库的类和函数
```java
public class ClusterManager {
    private Map<String, Cluster> clusterMap = new HashMap<>();

    public void setCluster(String clusterId, Cluster cluster) {
        synchronized (ClusterManage.class) {
            clusterMap.put(clusterId, cluster);
        }
    }

    public Cluster getCluster(String clusterId) {
        synchronized (ClusterManage.class) {
            return clusterMap.get(clusterId);
        }
    }

    public Map<String, Cluster> getAllCluster() {
        return clusterMap;
    }

    public void clean(){
        synchronized (ClusterManage.class) {
            clusterMap.clear();
        }
    }
}
```

完全等价于下面的代码，ConcurrentHashMap的性能还要比上面的快许多。

```java
public class ClusterManager  {
     private Map<String, Cluster> clusterMap = new ConcurrentHashMap<>();
     ...
}
```

再如将流变为String的代码：

```java
public static String inputStream2String(InputStream is) throws Exception {
	ByteArrayOutputStream bos = new ByteArrayOutputStream();
	try {

		int i = -1;
        while (( i = is.read() ) != -1) {
			bos.write(i);
		}
	} catch (Exception e) {
		throw new Exception(EXCEPTION_TYPE.READ_FILE_ERROR);
	} finally {
		if (is != null) {
			try {
				is.close();
				} catch (IOException e) {

                }
            }
        }
        return bos.toString();
    }
}
```

可以使用apache提供的commons.io替换。另外上面的代码还破坏了谁申请谁释放的原则。

```java
public static String inputStream2String(InputStream is) throws Exception {
    try{
        IOUtils.toString(is);
    } catch(Exception e) {
        throw new Exception(EXCEPTION_TYPE.READ_FILE_ERROR);
    }
}
```
甚至你可以连这个函数都不写了。直接调用IOUtils.toString()。


### 3.11 多歧路，今安在

下面的代码if中的判断条件太多。

```java
if (((optLocal.immediate != 0) &&
             ((optLocal.immediate == 1) ||
              (difftime(now(), srun_begin_time) >=
               optLocal.immediate))) ||
            ((rc != ESLURM_NODES_BUSY) && (rc != ESLURM_PORTS_BUSY) &&
             (rc != ESLURM_PROLOG_RUNNING) &&
             (rc != SLURM_PROTOCOL_SOCKET_IMPL_TIMEOUT) &&
             (rc != ESLURM_INTERCONNECT_BUSY) &&
             (rc != ESLURM_DISABLED))) {

                 // do something
             }
```

改为：

```java
long intervalTime = difftime(now(), srun_begin_time)
if (isExpired(intervalTime, optLocal.immediate) || isSlurmdConnectedOK(rc)) {

}
```


### 3.12 云深不知处

嵌套太深不行呀。这里是一个通信协议解析的例子。下面是循环解析的函数

```java
private void loop() {
    while(true) {
        try {
            if (isConnected()) {
                Cmd cmd=null;
                while((cmd = read())!=null) {
                    switch(cmd.getCmd()) {
                        case "PING": {
                            writePong();
                        }
                        break;
                        default:
                        break;
                    }
                }
            } else {
                connect();
            }
        } catch(Exception) {
            cleanConnection();
            reConnect();
        }
        
    }
}
```

这里代码函数幸好不长。如果长且分支多，那读代码就很容易迷失自我。应考虑拆分成多个函数。

```java
private void loop() {
    while(true) {
	    parse();        
    }
}

private void parse() {
    try {
        if (!isConnected()) {
            throw Exception(...);
        }
        Cmd cmd=null;
        while((cmd = read())!=null) {
            processCmd(cmd);
        }
        
    } catch(Exception) {
        cleanConnection();
        reConnect();
    }
}

private void processCmd(Cmd cmd) {
    switch(cmd.getCmd()) {
        case "PING": {
            processPing();
        }
        break;
        default:
        break;
    }
}

private void processPing() {
    writePong();
}

```

### 3.13 己之长，彼之短
以Java为例，这是一种面向对象的语言。面向对象并不是main必须放在某个类中这么狭义，需要有一切皆对象的观念才行。对象才是主题，代码总是围绕"对象做了什么事"来发展。如果你用C写面向对象的代码：就要写结构体，在结构体里再用函数指针。那代码看起来不舒服，写起来也不舒服。在Javascript中函数才是一等公民，要用函数式的想法去写代码。


### 3.14 数据放在哪里？
是使用关系型数据库存储还是key-value存储？是集中存放还是分布式的？上线/发布后数据量规模？这些是否都在代码里体现了？是否有遵循范式、是否有索引加速、列是否设计的恰当、主外键设置、有没有离线查询分析的优化设计、针对业务量增大后的数据划分处理，旧的数据如何兼容等。

### 3.15 知难,行亦难
代码写的复杂，那有没有写mock testcase? 是否输入的数据集都有考虑？单元测试有没有覆盖大部分分支？循环分支测试时0循环和1循环是否都测试到了？if分支是不是每一个谓词都测试到了？


# 第四章 推动公司层面进行Code Review

首先的首先，你要推动某件事情，你一定要先成为一个坚定的人，一个让人信服的人。那大家才会觉得你推动去做的事可行。这不是一朝一夕的事情。如果公司的大部分研发没有时间进行Code Review，这将是一件极为困难的事情。因为困难所以值得认真去做。

我们不能考虑以下场景：
    
    你振臂一呼：咱们Code Review吧。
    研发人员纷纷表示赞同。
    然后没两天就搭建了一个Code Review平台。
    然后大家就顺顺利利开始Code review了。
    老板高层纷纷表示祝贺。
    
这不叫你推动了Code Review. 这只能叫你赶上了好时候。因为没有你的振臂一呼，其他虾兵蟹将也能振臂一呼，只是早晚而已。所以这种事情太顺利，顺利到高层基层都统一意见：觉得这件事可行。还是那句话：你只是赶上了好时候。我们不讨论那么顺利的局面。我想说的是如何从最不利的局面开始推动。

## 4.1 战略篇

推动公司层面Code Review的战略思想是：

```
利益为先、攻心为上、权谋次之、政令最下
```

### 利益为先：没有永远的朋友，只有永远的利益
最为重要的一条：利益为先。公司里做事情，大家的目标就是完成KPI，而完成KPI在老板那里就是实现净利。每个公司都应该是盈利的公司，不管是现在盈利还是将来盈利。从来没有公司以不盈利为荣，因为根本就不值得炫耀。你要做的事情，首先要符合“赚钱”这一条基本原则。虽然Code Review的本质是提高效率、提高开发质量、减少成本，最终赚钱。但是Code Review投资回报周期特别长，大家特别是*老板和投资人*看不到这么深远，也没有那么多耐心。所以Code Review只能是辅助盈利的手段。先摒弃掉技术公司一定要Code Review的想法。如果同事们加班加点都不能在deadline之前完成项目交付/上线赚钱, 那说明他们的时间已经100%用在直接赚钱上了。那你能做的事就是等待，千万别提Code Review。 


### 攻心为上：让每个人都想Code Review
每个人都忍住不去Code Review。当你振臂一呼的时候，事情就成了。如果每一个人都知道Code Review的好处，每一个人都感受到Code Review的甜头，无往不利。

### 权谋次之：求好心人帮帮你吧
希望有好心的Lead/CTO可以帮你，那就先和他们打好关系吧。在他们困难时出手相助，并解决问题。事后再宣传一下Code Review的妙处。用不了太久大家就会想尝试一下Code Review. 大家虽然带着尝试的心，但实践效果不错。这样大家就能坚定不移的进行Code Review.

### 政令最下：如果你是CTO/Team Lead
宣布从今天起，所有提交的代码都必须经过Code Reivew. 首先大部分人会质疑，但是你身处高位，可能不会有人对你说不不不不。但是反抗的心似乎已经埋下。如果结果稍稍不好，可能怨声载道。不过如果结果是好的，属下们还是会高呼“英明领导”。

战略思想必须贯彻始终。需要大家摆正心态，不要在意一时的得失。即使现在无法推动Code Review，不代表以后没有机会。下面的章节将介绍推动的阶段和每个阶段对应的策略。

## 4.2 阶段篇

我们将推动Code Review这件事情划分为四个阶段。这四个阶段一定是顺序发展的。但是每个阶段又可能直接退化到第一阶段。我称之为“逆水行舟，不进则退”。
<div align=center>

![review\_pharse](/images/code-review/review_pharse.png)

</div>
### 准备阶段
这个阶段要注意三点：培训、播种、除草。这一阶段的重点是让大家或多或少的知道Code Review；激发少部分人尝试Code Review的欲望；削弱反对派的声音。当某个团队中大部分都是赞同派时就可以开始下一阶段。

*培训* 

首先看公司的情况。如果可以在全公司范围培训的。尽可能多培训几次。培训的主题抛砖引玉：首先如何写高质量的代码云云,然后再培训如何改进自己的代码云云,最后培训如何review别人的代码。一是宣传自己，二是让大家知道原来代码还可以这么写，三是培养一下自己的演讲技巧，四是减缓反对Code Review的情绪，五是增强赞同派的信心。所以多做培训总是有好处的。

*播种* 

* 有一部分人可以通过培训成为种子。
* 你也需要找一些有力的帮手。这些帮手应该和你都是君子之交：平时点头问候、也不礼尚往来、也无直接利害关系。但是他们有应该有一个共同点：必须是各个团队的技术负责人或者技术最牛的人。你和他们怎么交流呢？抱着虚心学习的心态用Code Review交流。看过这些人的代码，你可能需要帮忙修复逻辑错误、优化代码。也可以和他们讨论一下设计/架构的问题。提出更好的解决方案，但也不能质疑别人之前的设计。这里review一定不能使用checklist。 你要是指出这些人的代码里多加了几个空格这种无伤大雅的问题。那你就别想继续推Code Review了。
* 培养应届生。他们是最容易成为种子的人。以后也会成为公司的中坚力量。
* 老板。你应该让老板知道不进行Code Review的危害。让他觉得我们应该找个时间尝试一下。

*除草* 

有些人极力反对Code Review。他们觉得这占用了他们的编码时间；也可能源自他们之前的尝试。这群人的技术水平可能参差不齐，但是工作年限一般较长。技术大牛觉得你们这群杂兵就不要review我的代码了，反正我也汲取不到什么营养，还可能从我这里偷学点什么。技术一般的老人觉得我的代码怎么能公开呢?万一写的不好，威信全无。你需要慢慢转化他们的思维。你需要通过Code Review帮他们找出代码*运行时*确实会出现的bug。慢慢的，他们会觉得这种方法可以提升他们的效率，减少他们排错的时间。记住三点：绝对不要提无伤大雅的修改意见；绝对不要让别人大面积修改代码（即使你觉得代码结构实在混乱）；绝对不要拿出你的checklist对照排查。这是一场温和的变革，不能意见不合就排挤/打压/遣散反对的人。总之一句话，*化敌为友*。我们需要将反对者影响的比例控制在某个阈值以下。


### 试点论证
现在已经有一个团队愿意尝试这件事了。这一阶段重要是让团队成员自发得出一条结论：

    Code Review有利可图。

这结论应源于：

1. 自身能力的提升。知道代码有哪些地方可以更优雅一些，能看出运行时错误，减少了debug时间，学习到编码技巧。
1. 团队效率的提升。顺利看懂团队写的所有代码，修复其他成员引入的bug。
1. 当自己请假时，有他人可以暂代你的工作。不会打扰你休假。
1. 代码稳健。不为线上bug奔波。

结论已出，会进入推广阶段。

### 推广阶段
从一个团队，到其他的团队。这并不是一件非常困难的事情。只要那些团队有时间，就有尝试Code Review的机会。另外团队间交流/协作必不可少。以Code Review方式交流代码，潜移默化相关的团队。这一阶段结束的标志是大部分研发团队尝试使用Code Review，并保持3个月。

这一阶段的重点在于*奖和速*。

奖励对Code Review的推动有贡献的人。迅速的推广传播也至关重要。公司不会有那么多时间等待你推广，这期间会有大量的变数（人员流动、团队重组、高层空降、KPI变更等等），所以越快越好。


### 巩固阶段
恭喜已经进入到了巩固阶段。这个阶段结束的标志就是研发团队进行review活动的比例和频率在持续下降。某些进行Code Review的团队已经放弃这一活动。

这一阶段的重点在于*罚*。

惩罚不愿意进行Code Review的人/团队。这一阶段可以形成规范的Code Review checklist。 可以帮助研发团队整齐划一的编写代码。而在其他几个阶段我都不建议使用checklist进行教条式的Code Review。


## 4.3 小结
想要在公司层面大范围推动某件事，首先要让人信服。对于Code Review来说，需要基层和高层都赞同才行。要让赞同派发声，也要让反对派闭嘴。在大面积推广之前，你应该小心谨慎，步步为营,增加研发团队的认同感。始终要灌输Code Review有利可图这一理念。因为是你主导此事，所以一定要亲力亲为，决不能眼高手低：不review代码，也不写代码。但世间之事，哪能努力后就一定成功？倘若风云变幻，打回准备阶段。那也不必灰心，大侠请从头再来。

如果你技术水平还未到平均水平，仍要强推Code Review. 要么修炼自身技术；要么煽动技术大牛去推动Code Review。


# 第五章 总结
第一章流程部分：多人review需要注意分工协作,但是多人也容易导致无人负责。这一章主要是建立代码交流的流程。至于怎么反馈建议，使用什么工具，怎么保证这个通道畅通，并没有限制。

Code Review技巧分了两个章节。希望阅读后能有所启发。阅读代码的方式和阅读文章相似。但是读文章并没有跳转、递归、循环。在Code Review的考察点章节，可能有你似曾相识的言论，那就权当加深印象。我所写的是在Code Review经常/反复出现的代码问题，供大家参考。如果编码者看完这两章后若有所思，就已经达到目的。

第四章公司层面推动。仁者见仁，智者见智。从最不利的局面开始，如何一步步推进Code Review. 某些在公司顺风顺水、一呼百应的人完全可以忽略本文。我希望高级研发工程师和技术系高层看完后，能打开新的思路推动Code Review. Code Review这件事，毕竟和我们的职业活动息息相关，希望大家重视。

*希望有人看过此文重燃起推动Code Review的念想*。
