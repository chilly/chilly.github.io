---
layout: post
title: "design pattern--composite pattern"
date: 2010-5-3
wordpress_id: 196
comments: true
tags: ["cc", "code", "code", "composite", "design", "pattern", "software-architecture-code"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="this pattern is composite pattern.in this pattern, it like a tree to call the leaves function.so there is an entry of high level (Graph), and each class has a function called "Draw" ,the composite class is "Picture".and the leaves are "Line" and "Point"." />
<meta name="_su_keywords" content="code,design,pattern,composite" />
<meta name="_su_title" content="design pattern composite pattern" />
<meta name="views" content="532" />

{% codeblock %}// composite.cpp : Defines the entry point for the console application.
//

#include&lt;iostream&gt;
#include&lt;string&gt;
#include&lt;vector&gt;

namespace pattern{
	/// this pattern is composite pattern.
	/// in this pattern, it like a tree to call the leaves function.
	/// so there is an entry of high level (Graph), and each class has a function called "Draw" 
	/// the composite class is "Picture".
	/// and the leaves are "Line" and "Point".
	/// This pattern is combine many objects together which have same functions 
	/// and have level structures .
	/// And the composite class is like a manager.
	/// It has the same function also. 
	/// Benefits of this pattern is : outer class will never know the composite relationship;
	/// outer class will deal the composite class only, not all of innerclass. and composite class
	/// and inner class have same functions or level structures.
	/// Do you think the composite class like a facade in facade pattern?
#ifndef GRAPH_H
#define GRAPH_H

	class Graph{
	protected:
		std::string name;

	public:
		Graph(std::string name):name(name){std::cout&lt;&lt;"graph "&lt;&lt;name&lt;&lt;" construct"&lt;&lt;std::endl;}

		virtual void Draw()=0;
		virtual ~Graph(){std::cout&lt;&lt; "graph "&lt;&lt;name&lt;&lt; " destroy"&lt;&lt;std::endl;};
	};
#endif

#ifndef PICTURE_H
#define PICTURE_H
	class Picture:public Graph{
	private:
		std::vector&lt;Graph*&gt; list;
	public:
		Picture(std::string name):Graph(name){std::cout&lt;&lt;"Picture "&lt;&lt;name&lt;&lt;" construct!"&lt;&lt;std::endl;}
		virtual void Add(Graph*);
		virtual void Remove(Graph*);
		virtual void Draw();
		~Picture();
	};

#endif
	Picture::~Picture(){
		list.clear();
		std::cout&lt;&lt;"Picture destroy!"&lt;&lt;std::endl;
	}
	void Picture::Add(pattern::Graph * g){
		list.push_back(g);
	}
	void Picture::Remove(pattern::Graph *g){
		std::vector&lt;Graph*&gt;::iterator i=list.begin();
		while(i!=list.end()){
			if(g==*i){
				list.erase(i);
				break;
			}
			i++;
		}
	}
	void Picture::Draw(){
		std::vector&lt;Graph*&gt;::iterator i=list.begin();
		while(i != list.end()){
			(*i)-&gt;Draw();
			i++;
		}
	}

#ifndef SHAPE_H
#define SHAPE_H
	/// Shape class is parent class of "Line" and "Point"
	class Shape:public Graph{
	public:
		virtual void Draw()=0;
		Shape(std::string name):Graph(name){std::cout&lt;&lt;"Shape construct!"&lt;&lt;std::endl;}
		virtual ~Shape(){std::cout&lt;&lt;"Shape destroy!"&lt;&lt;std::endl;}

	};

#endif

#ifndef POINT_H
#define POINT_H
	class Point:public Shape{
	public:
		Point(std::string name):Shape(name){std::cout&lt;&lt;"Point construct!"&lt;&lt;std::endl;}
		~Point(){std::cout&lt;&lt;"Point destroy!"&lt;&lt;std::endl;}
		virtual void Draw();

	};
#endif
	void Point::Draw(){
		std::cout&lt;&lt;"Draw a point!"&lt;&lt;std::endl;
	}

#ifndef LINE_H
#define LINE_H
	class Line: public Shape{
	public:
		Line(std::string name):Shape(name){std::cout&lt;&lt;"Line construct!"&lt;&lt;std::endl;}
		~Line(){std::cout&lt;&lt;"Line destroy!"&lt;&lt;std::endl;}
		virtual void Draw();
	};

#endif
	void Line::Draw(){
		std::cout&lt;&lt;"Draw a line!"&lt;&lt;std::endl;
	}
}

int main()
{
	pattern::Line l ("line");
	pattern::Point p1 ("point 1");
	pattern::Point p2("point 2");
	pattern::Picture pic("Picture");
	pic.Add(&amp;l);
	pic.Add(&amp;p1);
	pic.Add(&amp;p2);

	pic.Draw();
	// if you want destroy some shape, remove them first.
	pic.Remove(&amp;p1);
	p1.~Point();
	pic.Draw();
	return 0;
}{% endcodeblock %}
 
