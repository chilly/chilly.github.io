---
layout: post
title: "design pattern -- decorator pattern"
date: 2010-5-3
wordpress_id: 198
comments: true
tags: ["cc", "code", "code", "decorator", "design", "pattern", "software-architecture-code"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="_su_keywords" content="design,pattern, decorator,code" />
<meta name="_su_title" content="design,pattern, decorator,code" />
<meta name="views" content="426" />

{% codeblock %}// decorator.cpp : Defines the entry point for the console application.
// author: chillyc
// email: chillycreator @t gmail.com

#include&lt;iostream&gt;
namespace pattern{
	/// this pattern is decorator pattern.
	/// In this pattern the super class should have default process functions.
	/// for example: "Read" has default read word function, and it is useful in the future.
	/// Read_Char, Read_Chinese, Decorator inherit Read class.
	/// But those three subclass is not powerful to deal with sentences.
	/// Then the subclass of Decorator will deal with sentences, but it dose not know every word.
	/// So Sentence(Encode) pattern has become.
	/// But decorator pattern is not only dealing with this situation but also enhance some classes
	/// capacity without adding new classes.
	/// is it something like Bridge?
#ifndef READ_H
#define READ_H
	class Read{
	public:
		Read(){}
		virtual ~Read(){}

		/// this is default process function.
		/// and decorator class will use this function to read
		virtual void ReadWord(){std::cout&lt;&lt;"default read!"&lt;&lt;std::endl;}
		/// if the function is a pure virtual function,
		/// decorator class need not inherit this class and
		/// it just has an instance of Read as a member varible.
		// virtual void ReadWord()=0;

	};
#endif

#ifndef READCHAR_H
#define READCHAR_H
	class Read_Char:public Read{
	public:
		Read_Char():Read(){}
		~Read_Char(){}
		virtual void ReadWord();
	};
#endif
	void Read_Char::ReadWord(){
		std::cout&lt;&lt; "read a char"&lt;&lt;std::endl;

	}

#ifndef READCHINESE_H
#define READCHINESE_H
	class Read_Chinese:public Read{
	public:
		Read_Chinese():Read(){}
		~Read_Chinese(){}
		virtual void ReadWord();
	};
#endif
	void Read_Chinese::ReadWord(){
		std::cout&lt;&lt;"read a chinese word"&lt;&lt;std::endl;
	}

#ifndef DECORATOR_H
#define DECORATOR_H
	class Decorator:public Read{
	protected:
		Read* r;
	public:
		Decorator():Read(){r = NULL;}
		Decorator(Read* r):Read(),r(r){}
		virtual ~Decorator(){if(r!=NULL) delete r;}
		virtual void ReadWord();
	};
#endif
	void Decorator::ReadWord(){
		if(NULL == r)
			Read::ReadWord();
		else
			r-&gt;ReadWord();
	}

#ifndef READSENTENCE_H
#define READSENTENCE_H
	// this class is real work decorator class
	class Read_Sentence:public Decorator{

	public:
		Read_Sentence():Decorator(){}
		Read_Sentence(Read* r):Decorator(r){}
		~Read_Sentence(){}
		virtual void ReadWord();
	};
#endif
	void Read_Sentence::ReadWord(){
		/// parent class method is called first!
		Decorator::ReadWord();
		/// then your method be called
		std::cout&lt;&lt;"read a sentence"&lt;&lt;std::endl;
	}
}

int main()
{
	pattern::Read_Sentence rs1(new pattern::Read_Char());// read char
	pattern::Read_Sentence rs2(new pattern::Read_Chinese());// read chinese
	pattern::Read_Sentence rs3; // default read
	rs1.ReadWord();
	rs2.ReadWord();
	rs3.ReadWord();
	return 0;
}{% endcodeblock %}
 
