---
layout: post
title: "Modify octopress blog"
date: 2013-07-21 02:41
comments: true
tags: octopress,blog,修改,结构 
categories:
- knowledge
---
 &nbsp;&nbsp;&nbsp;&nbsp;这几天一直在捣鼓blog.因为很久没有用**Octopress**写blog.另外也没有碰HTML相关。所以写markdown或者改Octopress都手生。这里把各种修改方法一一记下。要学习的是**ruby**, **Liquid**.  

<!-- more -->

## 0.开始学习

 &nbsp;&nbsp;&nbsp;&nbsp;学习ruby推荐以下这几个网站：  

 *  [ruby doc](http://ruby-doc.org/ 'http://ruby-doc.org/')
 *  [ruby中文的官方文档](http://www.ruby-lang.org/zh_cn/documentation/)
 *  [ruby 在线运行环境](http://www.compileonline.com/execute_ruby_online.php) 使用这个可以在线运行ruby. 查看执行结果。对于机器没有装ruby,这个是极好的平台。  

 &nbsp;&nbsp;&nbsp;&nbsp;然后是学习Liquid. **Liquid**这个本身是用ruby写的。所以只需要会用就行了。语法很像JSP.   
![Liquid语法example](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/671FD472DEC849C3B7B9F94A6A67DD6B)

 &nbsp;&nbsp;&nbsp;&nbsp;YAML. 这个全称是'**YAML Ain't Markup Language。**'。定义看起来和GNU很想。这个鬼东西不希望采用XML的方式。基本是做配置文件使用的。如果想细细学习。[猛击这里](http://zh.wikipedia.org/zh-cn/YAML)。在Octopress中基本上也就是定义了一下全局的某些量的true,false.其他基本上没有用。   

## 1. 开始修改Octopress blog

### 1.1 Octopress 简单介绍

 &nbsp;&nbsp;&nbsp;&nbsp;Octopress. 这个是继承了Liquid.然后加了一些plugins.最主要的plugin是plugins/octopress\_filters.rb. Octopress的工作原理就是使用Liquid对各种文档进行渲染生成html. 只不过比原始的Liquid的功能更多。另外使用Rake对各种命令打包。Octopress最重要的几个目录是: source, plugin. Octopress会先进行渲染，然后将渲染后的文件放入到public目录中，之后再将public目录中的文件移动到\_deploy目录下。然后通过git上传\_deploy目录完成发布。所以如果你想加入什么自己新写的html不要放到public或者\_deploy目录下。要放在source目录完成发布。所以如果你想加入什么自己新写的html不要放到public或者\_deploy目录下。要放在source目录下。比如我为自己的blog自定义了一个404.html.那么就要放入到source目录下。或者放入其他目录下并且改写Rakefile, 改写 “**multitask :push**”中的命令。   

### 1.2 Octopress source的文件目录结构。   

 *  首先是\_posts这个目录。这个是用来存放你用markdown写的blog的。使用rake new\_post['xxxx'] 就可有创建一篇名为xxxx.markdown的文件。
 *  \_includes这个目录。这个目录是引用各种模板html的。例如我的jiathis脚本和google ads脚本.我个人一般会将这些第三方存放在\_includes/post目录下。引用的时候使用{&#37; include post/xxx.html &#37;}. 如果是在\_include下的article.html,引用则为{&#37; include article.html &#37;}. 其中最重要的是article.html文件。这个每篇文章的显示。并且对于首页和具体blog都是引用的article.html. article.html中有 **{&#37; if index &#37;}**，如果index为true,则是首页。
 *  \_layouts这个目录，这个目录是对应着每篇文章的样式(当点进某篇文章后，这个目录下的文件就开始其作用)。其中最重要是post.html这个文件。修改也主要是修改这个。
 *  index.html. 这个就是首页了。页面的排版都在这里。
 
## 2. 实战  

### 2.1 jiathis 添加
 &nbsp;&nbsp;&nbsp;&nbsp; 下面以jiathis的添加为例。  
![jiathis 图标](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/412D2139E39C487BAA77D2E43A2A442C)   
 &nbsp;&nbsp;&nbsp;&nbsp; 加入jiathis需要修改以下这些文件：  

 * 添加 source/\_include/post/jiathis.html （不用照抄，直接在jiathis.com中找找同样的html即可。）  
![jiathis.html](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/4182C91E8C5F4F7C833C251570D109E4)    

 * source/\_include/article.html 修改为：   
![jiathis 放入article.html的位置](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/25EE2B2EE60D49D5868663E6BD80C2F4)
  图中修改的位置是点击到详细页面后，jiathis出现。如果希望在首页就出现，一种可以修改 else之前的，或者直接修改source/index.html文件  

### 2.2 修改侧边栏
  &nbsp;&nbsp;&nbsp;&nbsp; 如果你的blog有侧边栏的需求的话。那么需要改两个地方：一个是post.html,另外一个是index.html. Octopress的页面控制基本就是由这两个文件组成。index.html是控制首页。而post.html是控制详细页。

 * source/\_layouts/post.html 修改最下方，例如下图(不要照抄)：  
![sidebar](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/A5AB8C677BCB45D09CA2ED0D43A359B8)

 * source/index.html 也是修改最下方。   

![index sidebar](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/B49102887AFD49238793A6D57F3365A6)

## 4. 添加404页面。

 &nbsp;&nbsp;&nbsp;&nbsp; github上的404页面可以定制。只需要在index.html那个目录中加入一个404.html即可。例如我的：   
![404.html](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/9518399117EE4D2BB4BD81B1548DC979)

 
## 3. 总结

  &nbsp;&nbsp;&nbsp;&nbsp; 之前觉得这个Octopress很cool,后来才发现上了贼船。这东西定制起来还是比较费劲。另外最费劲的就是Octopress升级。下一章讲如何升级。不过还是劝大家暂时不要升级，下个版本感觉不稳定，有很多诡异的问题产生，而且它的兼容性也没有做好。还有就是在markdown里写Liquid脚本。这才是个渣。因为有时候希望这些脚本被显示的展示给用户，那么就需要将这些脚本转为16进制，否则Liquid就会解析该代码。所以用16进制写{&#37;&#37;}，或者直接使用截图的方法。另外Octopress的双引号解析更渣，都解析反了...所以推荐使用单引号。


 
