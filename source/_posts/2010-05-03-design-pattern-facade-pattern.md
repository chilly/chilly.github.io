---
layout: post
title: "design pattern -- facade pattern"
date: 2010-5-3
wordpress_id: 202
comments: true
tags: ["cc", "code", "code", "design", "facade", "pattern", "software-architecture-code"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="software architecture
design pattern of facade" />
<meta name="_su_keywords" content="facade,pattern,code,design" />
<meta name="_su_title" content="facade,pattern,code,design" />
<meta name="views" content="417" />

{% codeblock %}// Facade.cpp : Defines the entry point for the console application.
// author: chillyc
// email: chillycreator @t gmail.com

#include&lt;iostream&gt;

namespace pattern{
	/// this pattern is facade pattern
	/// facade pattern is for hiding relationship and behavior of inner classes .
	/// client or outer class should only connect with facade class.
	/// But it will not forbid accessing inner classes.
#ifndef INNERA_H
#define INNERA_H
	class Inner_A{
	public:
		void Do(){std::cout&lt;&lt;"A"&lt;&lt;std::endl;}
	};
#endif
#ifndef INNERB_H
#define INNERB_H
	class Inner_B{
	public:
		void Do(){std::cout&lt;&lt;"B"&lt;&lt;std::endl;}
	};
#endif

#ifndef FACADE_H
#define FACADE_H
	class Facade{
	private:
		Inner_A* a;
		Inner_B* b;
	public:
		Facade(){
			a = new Inner_A();
			b = new Inner_B();

		}
		void Do(){
			a-&gt;Do();
			b-&gt;Do();
		}
		~Facade(){
			delete a;
			delete b;
		}
	};
#endif
}

int main()
{
	pattern::Facade f;
	f.Do();
	return 0;
}{% endcodeblock %}
 
