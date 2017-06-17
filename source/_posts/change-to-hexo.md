---
title: 博客迁移史
date: 2017-06-17 15:24:31
tags: ['hexo','blog','Octopress','theme','Jekyll','wordpress']
---

# CSDN 时代
最初写blog时，是在大学的时候。那时都是主站点的天下。程序员杂志的影响力巨大。以程序员杂志起家的CSDN自然是程序员写作的首选。当时CSDN的竞争对手有JAVA eyes, 博客园，新浪微博等等。但是在编程方面影响力最大的还是CSDN. 所以当时在CSDN上开创了blog, 起名帐前卒。一写就是数十年。主要是记录自己编码生涯的点点滴滴。除了如何进行编码和debug，就是电影看书、吃喝拉撒，当然也还有自己对生活的理解。再看之前的博文，似乎又回到了那个疯狂编码的年代。所以写blog其实不是给别人看的，最主要是给自己看的。只不过顺便传道授业解惑。不过说到解惑，其实自己现在仍然还很困惑。解人之惑，却解不了自己之惑。

当初CSDN的blog是可以自己添加一些计数器的js脚本的。但是好景不长，总有恶人做恶，写些木马js什么的。所以CSDN就全部封禁了博主的js. 当时我并没有觉得什么不妥，只不过在自己的blog页面缺失了一个漂亮的计数器。

不过几十年后的今天，CSDN对博客专家重新开启了自定义脚本。虽然CSDN已向前迈出一步，虽然我已是博客专家，但对此功能已经丧失了兴趣。

# WordPress时代
## 迁移
后来godaddy来了。可以申请域名了。不用再需要什么身份证、户口本、地址、电话才能申请个域名，省却了系统备案的时间和麻烦。于是就申请了chillyc.info. 为什么申请这个域名呢？嗯，其实我也是稀里糊涂的申请的。


我们当时几个人一起买了一个主机。主机上有我们几个人的wordpress搭建起来的blog。需要自己通过ftp上传各种php的plugins。需要自己作为admin编写博客，改写某些plugin。但是好景不长，很快就被黑掉，并植入了病毒。另外也需要主机继续购买续费。所以后来就放弃了这个wordpress搭建的blog。



但是搭建这个blog还是有很多的收获。首先有了自己的Google AD，后来使用自己申请的Google Analytics的获得的blog访问数据写了一篇论文。可惜后来Google被墙。AD和Analytics都需要翻墙。Google当时的这些服务又都是同步加载的。这导致blog加载的速度非常非常的慢，看客也就慢慢的流失了。不过当时并没有花主要精力在chillyc.info上。


Wordpress搭建blog站点的最大的问题就是没有预览功能。那意味着你需要搭两个同样的wordpress服务，一个线上的一个线下的。然后再改改看看。另外当时还没有搭svn或者git，服务代码同步起来特别麻烦。

# Jekyll 与 Octopress

再后来有了github。我开始准备将wordpress中的内容迁移到Octopress上。这是一个使用ruby写的服务。将markdown的内容转变为静态html.然后在将内容传送到github上。最终通过github的域名解析变成自己的域名。当时Octopress也提供了从wordpress到octopress的迁移程序。总之搞了一段时间终于将blog从wordpress迁移下来。其实自己并不喜欢markdown文法，虽然简单，但是没有wordpress的富文本编辑器简单。主要没有鼠标点点，所以写blog的效率还是下降了不少。还好后来渐渐习惯了markdown.


有了自己的github库，最终将过去再chillyc.info中的内容全部提交到了github上。自己也在Octopress的基础上修修改改，比如将url从utf-8的汉字编码修改为了中文拼音。这种改变其实没有提高什么google rank啥的。只是自己看着舒服了些。又如将页面样式修改为动态的，添加了一只追着骨头跑着的狗。这改变增加了不少blog的加载时间。


Octopress的问题在于首先要来一套ruby全家桶。其次生成页面的速度实在慢的要死要死的，比八爪鱼爬的还要慢。


再后来我也懒得管这个blog了。因为毕竟没有什么访问量。索性还是花精力在CSDN上。


# Hexo 


从2015年开始因为V8+ES6+node,以及facebook的JS解析器啥的。JS再次火了起来。大量的JS写的服务。这次blog生成工具是hexo. 一个nodejs写成的服务。原理和Octopress一样。只不过语言换了换。


因为自己许多年没有在动chillyc.info上的blog. ruby全家桶的搭建自己也忘记的差不多了。又换了新的电脑。一切将要重新开始。那为什么不试试新的服务呢？于是使用的hexo. 很幸运的是hexo沿用了Octopress的设计。宣称简单到只要将source目录copy过来即可。我非常心动。于是下载了一套JS的全家桶，开始Hexo的旅程。


但是这旅程实在不顺利。之前Octopress上好好的markdown,到了Hexo上却总是报错。费劲千辛万苦改正Hexo认为合法的markdown。下一步就是找一个合适的theme. 有人推荐了pacman这个主题。demo看起来很是不错。使用了一下。发现文章分类乱七八糟。看了Hexo的官方文档才发现。Hexo 并不支持一篇文章多分类。这和Octopress的假设是冲突的。于是编写脚本，将分类转变为了tags。这样blog看起来好看多了。我们的blog里面还有latex。发现pacman支持的也不错。只不过需要自己先手工把过去的latex改成适合Mathjs的形式。程序员何苦为难程序员。
