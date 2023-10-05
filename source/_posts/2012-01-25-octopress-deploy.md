---
layout: post
title: "octopress deploy"
date: 2012-01-25 23:23
comments: true
tags: [octopress, principle] 
categories:
- knowledge
---
octopress这东西还是很难弄的。这几天发生了各种git的异常。也不知道是怎么发生的。亦难解决。这stack overflow中查到都是中看不中用的东西。最后只好从github上再次clone下来。然后`git init`.这里要注意的是，octopress的home目录中其实是ignore `_deploy` 文件夹的。home 对应的是自己的source分支。而`_deploy`中才是master分支。      
`rake deploy` 其实是调用的`rake push`。其实是将`_public`中的内容copy到`_deploy`中。然后    
{% codeblock %}
cd _deploy
git init
git add .
git add -u
git commit -m "some time"
git push -u origin master
{% endcodeblock %}    
所以还是多看看octopress的内部实现比较好。
