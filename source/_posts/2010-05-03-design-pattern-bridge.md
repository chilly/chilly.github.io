---
layout: post
title: "design pattern--bridge"
date: 2010-5-3
wordpress_id: 193
comments: true
tags: ["bridge", "cc", "code", "design", "pattern", "software-architecture-code"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="why use bridge pattern?If the original parent class is not suitable for being inherited by subclass, it will be refactor. And If there are some concepts are common, it will be abstracted as implement interface or abstract class." />
<meta name="_su_keywords" content="design,pattern,bridge" />
<meta name="_su_title" content="design pattern bridge pattern" />
<meta name="views" content="465" />

{% codeblock %}// bridge.cpp : Defines the entry point for the console application.
//
/// author : chillyc
/// email : chillycreator @t gmail.com
/// uml:
///                   _ 
///                  / \
///                 /_ _\

#include&lt;iostream&gt;

namespace pattern{

/// why use bridge pattern?
/// If the original parent class is not suitable for being inherited by subclass, 
/// it will be refactor. And If there are some concepts are common, 
/// it will be abstracted as implement interface or abstract class.
/// for example: in KFC, there will be some softdrink to be sold, 
/// like coca cola, coff, and so on. And if you wish, you will add some additions like sugar,
/// or ice into softdrinks. If we design the classes as sugar-cocacola, sugar-coff, ice-cocacola,
/// and ice-coff. There will be four subclasses here. And in the future, is there add another
/// additions? If yes, the system will add another two classes! But if use bridge pattern,
/// it will add only one class called "another additions" inherited from addition class. 
/// This is benefits of this pattern.

#ifndef ADDITION_H
#define ADDITION_H
	/// this is the implement class in Bridge pattern.
	/// if you want add another additions, 
	/// inherit this class and the point of your class into Softdrink class.
	/// Do you have better implements for just adding class here and not modifying Softdrink class?
	class Addition{
	public:
		virtual void Add()=0;
		virtual ~Addition(){};
	};
#endif

#ifndef SUGAR_H
#define SUGAR_H
	class Sugar:public Addition{
	public:
		virtual void Add();
	};
#endif
	void Sugar::Add(){
		std::cout&lt;&lt;"add sugar"&lt;&lt;std::endl;
	}

#ifndef ICE_H
#define ICE_H
	class Ice:public Addition{
	public:
		virtual void Add();
	};
#endif
	void Ice::Add(){
		std::cout&lt;&lt;"add ice"&lt;&lt;std::endl;
	}

#ifndef SOFTDRINK_H
#define SOFTDRINK_H
	/// this class is the abstract class in Bridge pattern.
	/// I consider there will be some other implement methods.
	/// I use Addition objects as member variables, 
	/// and construct and destroy them in Softdrink.
	/// But if there is no member variables of Addition, It will be ok.
	/// for example: you can send Addition instances as parameters into Softdrink subclass.
	/// But in Bridge pattern, the uml is aggregation between implement class and abstract class.
	class Softdrink{
	public:
		Softdrink();
		virtual void drink()=0;
		virtual ~Softdrink()=0;
	protected:
		Addition *sugar;
		Addition *ice;
		/// another additions add here!
	};
	Softdrink::~Softdrink(){
		if(NULL != sugar){
			delete sugar;
		}
		if(NULL != ice){
			delete ice;
		}
	}
	Softdrink::Softdrink(){
		sugar = new Sugar();
		ice = new Ice();
	}
#endif

#ifndef COFF_H
#define COFF_H

	class Coff: public Softdrink{
	public:

		virtual void drink();

	};
#endif
	void Coff::drink(){
		std::cout&lt;&lt;"I am drinking coff!"&lt;&lt;std::endl;
		sugar-&gt;Add();
	}

#ifndef COCA_H
#define COCA_H
	class Coca: public Softdrink{
	public:
		virtual void drink();
	};
#endif
	void Coca::drink(){
		std::cout&lt;&lt;"I am drinking coca cola!"&lt;&lt;std::endl;
		ice-&gt;Add();
	}

}

int main()
{

	pattern::Coca coca;
	coca.drink();
	pattern::Coff coff;
	coff.drink();
	return 0;
}{% endcodeblock %}
 
