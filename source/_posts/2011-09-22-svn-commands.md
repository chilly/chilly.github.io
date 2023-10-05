---
layout: post
title: "svn commands"
date: 2011-9-22
wordpress_id: 508
comments: true
tags: ["project", "svn"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="views" content="141" />
<meta name="_wp_old_slug" content="svn-command" />
svn merge is used for merging code from branch or other path to chunk. Or you can use old version to cover new version.
svn co/ svn checkout is commonly used for checking code to local with svn information.
svn import is used for importing code to svn, which has no svn information.
svn export is used for checking out code without svn information.
<!--more-->
svn merge 用于将分支代码迁回到trunk,当然也可以将旧代码覆盖新代码
svn co 这个是最常用的，用于将代码迁到本地
svn import 用于将某些代码或者项目放入到svn server上
svn export 是将代码迁出来，但是迁出来的代码是没有svn信息的
