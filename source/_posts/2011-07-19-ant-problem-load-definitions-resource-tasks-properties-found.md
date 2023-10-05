---
layout: post
title: "ant problem: Could not load definitions from resource tasks.properties. It could not be found."
date: 2011-7-19
wordpress_id: 474
comments: true
tags: ["ant", "error", "open-source", "problem", "project"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="solve ant problem: Could not load definitions from resource tasks.properties. It could not be found." />
<meta name="_su_keywords" content="ant,problem,tasks.propertise,resource,load,not,error" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="ant,problem,tasks.propertise,resource,load,not,error" />
<meta name="views" content="400" />
<meta name="_wp_old_slug" content="ant-problem" />
problem is :
Could not load definitions from resource tasks.properties. It could not be found.
error code in build.xml
<div><strong> 
{% codeblock %}&lt;path id="xx.classpath"&gt;
   &lt;fileset dir="lib"&gt;
      &lt;include name="a.jar" /&gt;
      &lt;include name="b.jar" /&gt;
   &lt;/fileset&gt;
&lt;/path&gt;
&lt;taskdef classpathref="xx.classpath" resource="tasks.properties" /&gt;{% endcodeblock %}
</strong></div>
<div><strong> </strong></div>
<strong>{% codeblock %} 

{% endcodeblock %}</strong>

correct code in build.xml:


{% codeblock %}&lt;path id="xx.classpath"&gt;
   &lt;fileset dir="lib"&gt;
      &lt;include name="${basedir}/lib/a.jar" /&gt;
      &lt;include name="${basedir}/lib/b.jar" /&gt;
   &lt;/fileset&gt;
&lt;/path&gt;
&lt;taskdef classpathref="xx.classpath" resource="tasks.properties" /&gt;{% endcodeblock %}
 
