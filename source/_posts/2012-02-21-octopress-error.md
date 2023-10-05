---
layout: post
title: "octopress error, rake generate"
date: 2012-02-21 20:00
comments: true
tags: ["SyntaxError", "octopress", "error", "rake generate"]
categories:
- knowledge
---
Today, I met the following problem. I wrote an article, and use `rake generate` to generate the site. Then the console reports:
{% blockquote %}
/home/chico/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/1.9.1/psych.rb:148:in `parse': couldn't parse YAML at line 3 column 17 (Psych::SyntaxError)
{% endblockquote %}
why?     
Then I found the reason:   
{% blockquote %}
  "\\" should not be written in title of your article. If you really want to write it, use "\\\\"
{% endblockquote %}
