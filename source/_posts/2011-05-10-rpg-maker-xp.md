---
layout: post
title: "RPG maker XP 制作的游戏无法运行"
date: 2011-5-10
wordpress_id: 399
comments: true
tags: ["dll", "game", "rmxp", "rpg", "rtp"]
categories:
- game
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="解决RPG maker xp制作的游戏无法运行的问题" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="RPG,maker,xp,dll,无法运行" />
<meta name="views" content="686" />
这个东东制作RPG还是不错的。不过我用不了那么多功能。此外层数太少了，还要学习Ruby。其实Ruby和python差不多。那帮家伙为啥不用python呢？
算了。学点语言也没有坏处。然后我就做起了RPG.
昨天终于做好了。至少可以在我的电脑上运行了。
结果放在宝的电脑上就杯具了。
提示应该是缺少RGSS102J.dll的错误，其实RGSS103J.dll的错误是一样的。就是安装过RPG maker的电脑上安装了RGSS102J.dll，而没有装过的缺少呗。其实根本不用到网上下载那些dll。只需要在系统盘的windows文件夹寻找就可以找到。之后把这个dll拷贝到游戏目录中即可。
但即使这样还是会有提示说：RGSS-RTP Standard 找不到咯～所以嘛，在RPG Maker XP中的游戏->选择RTP中不要选Standard。
解决方案：
	<li>首先就在RPG maker XP的安装目录\RGSS\Standard中将Audio和Graphics两个目录拷贝出来。然后将你制作的游戏目录中的Audio和Graphics覆盖到这两个目录。然后再将这两个目录覆盖到自己的游戏目录。<strong>注意这里谁先覆盖谁。</strong></li>

	<li>第二步将游戏->选择RTP中全部改为Standard.如果你第一步做对了，那么现在你的游戏画面还是存在的，否则你就会发现全部都是空白。</li>

	<li>然后将RGSS102J.dll或者RGSS103J.dll放入到你的游戏制作目录。然后将这个dll文件的属性改为隐藏。不隐藏的话，RPG maker XP是不给打到安装包里的。</li>

现在制作打包，就可以在没有安装过RPG maker XP的系统中运行了。当然需要是windows的系统。
