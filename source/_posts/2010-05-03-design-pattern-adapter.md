---
layout: post
title: "Design pattern -- Adapter"
date: 2010-5-3
wordpress_id: 191
comments: true
tags: ["adapter", "cc", "code", "design", "pattern", "software-architecture-code", "virtual"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="adapter pattern is used for combining two or more classes,which have the same behaviors but different interfaces." />
<meta name="_su_keywords" content="design,pattern,virtual,class,constructor,destory,adapter" />
<meta name="_su_title" content="design pattern adapter pattern" />
<meta name="views" content="546" />

{% codeblock %}// Adapter.cpp : Defines the entry point for the console application.
// author: chillyc
// email: chillycreator @t gmail.com

#include&lt;iostream&gt;
#include&lt;string&gt;
using namespace std;

/// adapter pattern is used for combining two or more classes,
/// which have the same behaviors but different interfaces.
/// some classes is common used in future, and the other which are special will never used.
/// common used classes should be public, and the other is private.
/// some classes which inherit adapter class will never use the special ones. 
/// adapter pattern is divided into two patterns.
/// one is belong to class structure, and the other is belong to object structure.

#ifndef INTERFACEA_H
#define INTERFACEA_H
/// Interface A is a common interface!
class Interface_A{
public :
	virtual void connect_com();
};
#endif
void Interface_A::connect_com(){
	cout&lt;&lt;"connect a"&lt;&lt;endl;
}

#ifndef INTERFACEB_H
#define INTERFACEB_H
/// Interface B is not a common interface, 
/// it will be used in some special area!
class Interface_B{
public :
	virtual void connect_B();
};
#endif
void Interface_B::connect_B(){
	cout&lt;&lt; "connect b"&lt;&lt;endl;
}

#ifndef ADAPTERCLASS_H
#define ADAPTERCLASS_H
/// this class uses private inheritance to construct adapter.
/// and it is a kind of class structure.
class Adapter_Class: public Interface_A, private Interface_B{
public:
	Adapter_Class();
	~Adapter_Class();
	virtual void connect();
};

/// this class uses private member variable to construct adapter.
/// and it uses object relationship.
class Adapter_Object: public Interface_A{
private:
	Interface_B * b;
public:
	Adapter_Object();
	~Adapter_Object();

	virtual void connect();
};
#endif

Adapter_Class::Adapter_Class(){
	cout&lt;&lt; "Adapter_Class construct"&lt;&lt;endl;

}
Adapter_Class::~Adapter_Class(){}
void Adapter_Class::connect(){
	Interface_A::connect_com();
	Interface_B::connect_B();
}

Adapter_Object::Adapter_Object(){
	cout&lt;&lt;"Adapter_Object construct"&lt;&lt;endl;
	b = new Interface_B();

}
Adapter_Object::~Adapter_Object(){
	delete b;
}
void Adapter_Object::connect(){
	Interface_A::connect_com();
	b-&gt;connect_B();
}

int main()
{
	Adapter_Class ac;
	ac.connect();
	Adapter_Object ao;
	ao.connect();
	return 0;
}{% endcodeblock %}
 
