---
layout: post
title: "ivy,ant,eclipse配置启动"
date: 2011-7-18
wordpress_id: 466
comments: true
tags: ["ant", "code", "eclipse", "ivy"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="ivy,ant,eclipse" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="ivy,ant,eclipse" />
<meta name="views" content="485" />
下面的XX是贵公司..zzz
首先先把公司的ivy+ant 都下了

然后去fountain.corp.XX.com去试试自己的用户名，密码

然后eclipse 3.6.2, jdk 1.6_14

将工程导入到自己的eclipse中。

进入eclipse 3.6.2 然后Project-&gt;Propertise-&gt;Builder-&gt;New一个新的antBuilder然后编辑那个ant,将main中BuildFile设置为build.xml

在Project-&gt;Resource中将Text file encoding改为UTF-8

然后将Base Directory设置为你的工程文件，在classpath中选中Ant Home（并点击Ant Home...）,并将其改为新的ivy+ant的目录。

在build.xml中添加


{% codeblock %}&lt;target name="check-exist"&gt;
   &lt;echo message="user home = ${user.home}" /&gt;
   &lt;echo message="user name = ${ivy.auth.username}" /&gt;
&lt;/target&gt;{% endcodeblock %}


eclipse中windows-&gt;show views-&gt;Ant，选中check-existence然后 Run As-&gt;Ant Build

用ant执行上面的代码将产生User Home,判断\${user.home}/ivy.properties文件是否存在，并且是否能读到ivy.auth.username和Password

创建一个ivy.properties（这里要看扩展名是否是properties，一定不能是txt）放入到User Home中。并且这个文本也必须是UTF-8的。

最好是用eclipse新建一个，然后写好后，直接将这个文件放入到User Home中

ivy.auth.username=[your username]
ivy.auth.password=[your password]

[your username]是放入到fountain.corp.XX.com中用户名

写完后在ant中执行check-existence，看是否能读到username和password
如果ok，选中resolve 然后 Run As-&gt;Ant Build

之后调整一下项目的格式，或者让别人将eclipse中的.classpath发给你，之后F5刷新即可。
