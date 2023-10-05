---
layout: post
title: "design pattern -- flyweight"
date: 2010-5-5
wordpress_id: 205
comments: true
tags: ["cc", "code", "code", "design", "flyweight", "pattern", "software-architecture-code"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="software architecture
design pattern " />
<meta name="_su_keywords" content="design,pattern,flyweight,code" />
<meta name="_su_title" content="flyweight,pattern,code,design" />
<meta name="views" content="904" />

{% codeblock %}// flyweight.cpp : Defines the entry point for the console application.
// author: chillyc
// email: chillycreator @t gmail.com

#include&lt;iostream&gt;
#include&lt;map&gt;
namespace pattern{

	/// this is flyweight pattern 
	/// In text processing, there will be thousands of hundreds of characters.
	/// If we construct too many this class instance, and 
	/// imaging this class will have many member variables, will the system be efficient? 
	/// So we just record the characters state, and not care about other part of character.
	/// we shall save a lot of main memories, shall we?
	/// If there are many characters, but  they are only in differnt place, we can reuse characters.
	/// So there are Fly_Manager to record the shared objects or states.
	/// All objects should be derived from Metadata class.
	/// Graphdata is no-shared metadata. And Character is shared metadata.
	/// In main function, you will see the difference between two of them.

#ifndef STATE_H
#define STATE_H
	class State{
	private:
		int font_num;
		int style;

	public:
		void Set_Font_Num(int num){font_num = num;}
		void Set_Style(int style){this-&gt;style = style;}
		int Get_Font_Num(){return font_num;}
		int Get_Style(){return style;}
		State(){}
		State(int num,int style){
			font_num = num; 
			this-&gt;style = style;
		}
		State(const State&amp; state){
			this-&gt;font_num = state.font_num; 
			this-&gt;style = state.style;
		}
		State* operator = (const State&amp; state){
			this-&gt;font_num = state.font_num; 
			this-&gt;style = state.style;
			return this;
		}
	};

#endif

#ifndef METADATA_H
#define METADATA_H
	class Metadata{
	public:
		virtual void show()=0;
	};
#endif

#ifndef character_H
#define character_H
	// share the basic information
	// here you should image character has a huge infomation, 
	// and state is only a small part of this class
	class character: public Metadata{
	private:
		State state;
	public:
		// set state to default state
		character():Metadata(),state(1,1){}
		~character(){}
		void Set_State(State&amp; state);
		virtual void show();

	};
	void character::Set_State(pattern::State&amp; state){
		this-&gt;state = state;
	}

	void character::show(){
		std::cout &lt;&lt; "write a character font is "&lt;&lt; state.Get_Font_Num()&lt;&lt; " style is "
			&lt;&lt; state.Get_Style()&lt;&lt;std::endl; 
	}
#endif

#ifndef GRAPHDATA_H
#define GRAPHDATA_H
	// not share 
	class Graphdata: public Metadata{
	private:
		State state;
	public:
		Graphdata(State &amp; state):Metadata(),state(state){}
		// font num is not used
		Graphdata(int style):Metadata(),state(-1,style){}
		~Graphdata(){};
		virtual void show();
	};

#endif
	void Graphdata::show(){
		// only use its own state
		std::cout&lt;&lt; "print a graph! style is "&lt;&lt;state.Get_Style()&lt;&lt;std::endl;
	}

#ifndef FLYMANAGER_H
#define FLYMANAGER_H
	class Fly_Manager{
	public:
		// suggest you should make it as private or protected
		std::map&lt;int,State&gt; characters;
		Fly_Manager(){}
		void Set_I(int i, State&amp; state);
		State&amp; Get_I(int i);
		~Fly_Manager(){}

	};
#endif
	void Fly_Manager::Set_I(int i, State&amp; state){
		characters[i] = state;
	}
	State&amp; Fly_Manager::Get_I(int i){
		return characters[i];
	}

}
int main()
{
	pattern::Fly_Manager fm;
	pattern::character c;
	pattern::State s1(1,2);
	pattern::State s2(2,3);
	pattern::State s3(3,4);
	fm.Set_I(0,s1);
	fm.Set_I(1,s2);
	fm.Set_I(2,s3);
	pattern::Graphdata gd1(2);
	pattern::Graphdata gd2(9);
	for(int i = 0; i &lt; 3; i++){
		c.Set_State(fm.Get_I(i));
		c.show();
	}
	gd1.show();
	gd2.show();
	return 0;
}{% endcodeblock %}
 
