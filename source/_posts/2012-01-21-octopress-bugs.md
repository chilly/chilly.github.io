---
layout: post
title: "octopress bugs"
date: 2012-01-21 21:19
comments: true
tags: [octopress, bug, solve] 
categories:
- knowledge
---
<meta name="Keywords" content="octopress,bug,solve"/>
<meta name="Description" content="The page will show many octopress bugs. And tell you how to solve it."> 

Today, I have met a bug of octopress. When you write your blog with number in categories, like "cattegories: [some keyword, 2012]".

When you type  `rake generate`. The command will report:    

{%blockquote%}
/plugins/category_generator.rb:109:in `block in write_category_indexes`;: undefined method `gsub`; for 2012:Fixnum (NoMethodError)
{%endblockquote%}   
It is caused by your number `2012`.
{%codeblock%}
vi plugins/category_generator.rb
{%endcodeblock%}   
Go to line `109`. and add a line code:
{%codeblock%}
category = category.to_s
{%endcodeblock%}  
Now, type `rake generate` again. It will be fine.
