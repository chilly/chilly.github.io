---
title: 博客迁移史
date: 2017-06-17 15:24:31
tags: ['hexo','blog','Octopress','theme','Jekyll','wordpress']
categories:
- prose

---

# CSDN 时代
![CSDN](/images/change-to-hexo/csdn.jpeg)


最初写blog时，是在大学的时候。那时都是主站点的天下。程序员杂志影响力巨大。以程序员杂志起家的CSDN自然是程序员写作的首选。当时CSDN的竞争对手有JAVA eyes, 博客园，新浪微博等等。但是在编程方面影响力最大的还是CSDN. 所以我当时在CSDN上开创了blog, 起名帐前卒。一写就是数十年。主要是记录自己编码生涯的点点滴滴。除了如何进行编码和debug，就是电影看书、吃喝拉撒，当然也还有自己对生活的理解。现在的我再回看之前的博文，似乎总能回想起那个疯狂编码年代的故事。所以说写blog其实不是给别人看的，是给自己看的。只不过顺便兼顾了传道、授业、解惑。不过说到解惑，其实自己现在仍然很困惑。解人之惑，却解不了自己之惑。


当初CSDN的blog是可以自主添加一些计数器的JS脚本的。但是好景不长，总有恶人做恶:写些木马脚本什么的。所以CSDN就封禁了这功能。 当时我并没有觉得什么不妥，只不过在我的blog页面上消失了一个漂亮的计数器。


后来CSDN首页改版，放入了更多的文章，即使自己的文章推荐上了头条，也再也找不到在哪里了。


后来CSDN允许博客专家推荐自己的文章上头条，在那之后，上头条的荣誉感和胜利感也就随之消失。


几十年后的今天，CSDN对博客专家重新开启了自定义脚本。虽然CSDN又迈出一步，虽然我已是博客专家，但对此功能已了无兴趣。


现在CSDN又要求手机验证了，我都不想登陆CSDN了，就更别说在CSDN写blog了。

# WordPress时代
![Wordpress](/images/change-to-hexo/Wordpress.png)



godaddy进入中国了。可以申请域名了。不用再需要什么身份证、户口本、地址、电话才能申请个域名，省却了系统备案的时间和麻烦。于是就申请了chillyc.info. 为什么申请这个域名呢？嗯，其实我也是稀里糊涂的申请的。


我们当时几个人一起买了一个主机。主机上有我们几个人的wordpress搭建起来的blog。需要自己通过ftp上传各种php的plugins。需要自己作为admin编写博客，改写某些plugin。但是好景不长，很快就被黑掉，并植入了病毒。另外也需要主机继续购买续费。所以后来就放弃了这个wordpress搭建的blog。



但是搭建这个blog还是有很多的收获。首先有了自己的Google AD，后来使用自己申请的Google Analytics的获得的blog访问数据写了一篇论文。可惜后来Google被墙。AD和Analytics都需要翻墙。Google当时的这些服务又都是同步加载的。这导致blog加载的速度非常非常的慢，看客也就慢慢的流失了。不过当时并没有在chillyc.info上花主要精力。


Wordpress搭建blog站点的最大的问题就是没有预览功能。意味着你需要搭两个同样的wordpress服务，一个线上的一个线下的。然后再改改看看。另外当时还没有搭svn或者git，服务代码同步起来也特别麻烦。

# Jekyll 与 Octopress
![Octopress](/images/change-to-hexo/Octopress.jpeg)
再后来有了github。我开始准备将wordpress中的内容迁移到Octopress上。这是一个使用ruby写的软件服务。它将markdown的内容转变为静态html，然后再将内容传送到github上，最终通过github的域名解析变成自己的域名。这不需要自己再购买主机，省下一些银子。当时Octopress也提供了从wordpress到octopress的迁移程序。总之搞了一段时间后，终于将blog从wordpress迁移下来。其实自己并不喜欢markdown文法，虽然简单，但仍然没有wordpress的富文本编辑器简单，而且还需要各种html转义。没有鼠标点点，用vi写blog的效率还是下降了不少。还好后来渐渐习惯了vi+markdown。


有了自己的github库，再将chillyc.info wordpress的文章内容全部提交到了github上。自己也在Octopress的基础上修修改改：比如，将url从utf-8的汉字编码修改为了中文拼音。这种改变其实并没有提高什么google rank啥的。只是自己看着舒服了些。又如，将页面样式修改为动态的，添加一只追着骨头狂奔的狗。这改变又增加了不少blog的加载时间。


Octopress的问题在于：
1. 首先要来一套ruby全家桶。
2. 其次生成页面的速度实在慢的要死要死的，比八爪鱼爬的还要慢。


再后来我也懒得管这个blog了。因为毕竟没有什么访问量。索性还是将精力花在CSDN上。


# Hexo 
![Hexo](/images/change-to-hexo/hexo.png)

从2015年开始因为V8+ES6+node,以及facebook的JS解析器啥的。JS再次火了起来。网上用JS写的服务越来越多。这次blog生成工具是hexo. 一个nodejs写成的服务。原理和Octopress一样。只不过语言换了换。


因为自己许多年没有在动chillyc.info上的blog。 ruby全家桶如何搭建自己已忘记的差不多了，又换了新的电脑。一切将要重新开始。那为什么不试试新的服务呢？于是使用的hexo. 很幸运的是hexo沿用了Octopress的设计。宣称简单到只要将source目录copy过来即可。我非常心动。于是下载了一套JS的全家桶，开始了前往Hexo的旅程。


但是这旅程实在不顺利。之前在Octopress上好好的markdown,到了Hexo上却总是报错。费劲千辛万苦改成Hexo认为合法的markdown。下一步就是找一个合适的theme. 有人推荐了pacman这个主题。这个主题的demo看起来很是不错。我使用了一下，却发现文章分类乱七八糟的。看了Hexo的官方文档才发现。其实是Hexo并不支持一篇文章多分类。这和Octopress的假设是冲突的。于是自己编写转换脚本，将分类转变为了tags。这样blog看起来就好看多了。我的blog里面还有latex。发现pacman支持的也不错。只不过需要自己先手工把过去的latex改成适合Mathjs的形式。程序员何苦为难程序员。为什么整那么多不同的格式?



好吧，搞定latex后，blog看起来不错了。而现在csdn上的登陆需要验证手机了。没有手机号是不是永远就不能登陆了？那我索性就把CSDN上这么多年的blog全部迁移到github上吧。找了一个Python写的csdn导出工具。把所有的CSDN的html文本转换成了markdown。然后放入到hexo。然后就hexo generate就卡死了，卡死了，卡死了...找了半天问题，发现还是因为blog篇数太多，不管hexo还是Octopress都没有对这些做过优化。写的blog越多，生成的就越慢。换句话说，不管hexo还是Octopress在github搭建这条路上都没有考虑到上万blog或者更多blog的情况。


后来去掉了pacman主题，速度立刻提升了不少，所以这个主题优化做的差。去掉pacman主题后，生成700篇文章从10分钟降到了1分钟。我现在使用的主题是next，性能优化的比较好。如果你只是尝鲜，或者你只是一时兴起要写blog，那随便什么主题都适合你。否则你就要仔细挑选一下主题了。我推荐next主题。虽然这个主题人用多了，就丧失了个性。但我还是推荐大家用一下。


搞定主题后，我又安插了Google AD和Google analytics。经过了那么多年，Google终于提供了异步加载的JS脚本。虽然我的网站访问量低，但是我仍然考虑着提升读者的访问速度。我不指望用Google AD赚到钱。但如果能赚到意外之财，我也会很开心。最近也出了简书等靠打赏存活的blog工具。但我觉得我用google AD赚到钱的概率应该比采用各种打赏的方式靠谱一些。



# 后记

为什么所有的github blog搭建方式都选择生成html? 如果直接写markdown,而在前端直接将markdown渲染成本应的样式，就没有必要把时间浪费在生成html上了。我觉得很大可能是github的原因。希望它未来会考虑将github blog做的更好。不过话又说回来，将这个功能做的更好，对它来说又有什么好处呢？除非它有更加宏伟的目标——将全世界的所有站点都迁移到github上。这样只用github搜索就足够了。不过显然这个目标太过宏伟了。


或许再过几年又会出来一款新的blog搭建工具。工具总是在推陈出新。所以折腾必不可少，计算机界有太多身后而身先的例子。或许某一天我们人类终将被历史淘汰，但我们也应坚持到那一天，不是吗？所以继续折腾吧。活到老就折腾到老吧。

