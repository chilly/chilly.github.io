---
layout: post
title: "trickies and bugs in c"
date: 2010-8-13
wordpress_id: 308
comments: true
tags: ["bug", "c", "cc", "code", "macro", "point", "tricky"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="views" content="542" />
the macro #define is the biggest hidden bug in c. You should always remember macro-function is like code paster. And it will slow down your system in some special situations, such as macro recursion. If you macro define some function use BIG characters.


{% codeblock %}#define FUN1(a,b) a*b
#define FUN2(a) a++;\
     a*=a;

int main(){
	int a = 2;
	int b = 3;
	FUN1(a,5+1); // =&gt; 2*5+1
        if(a&gt;2)FUN2(a); //=&gt; if(a&gt;2) a++;a*=a;

	return 0;
}{% endcodeblock %}


strcpy() will copy "\0", and it is dangerous when you copy two points.


{% codeblock %}int main(){
	char str1[] = "abcd";
	char str2[] = "12";
	strcpy(str1,str2); // ok, str1 equals "12"
	char *st1 = "abcd";
	char *st2 = "12";
	strcpy(st1,st2); //crash!! st1 points to static const string
}{% endcodeblock %}


param as point or array style. Although they are differnt styles, they are the same.


{% codeblock %}int size1(char a[]){
	cout &lt;&lt; sizeof(a);
}
int size2(char* a){
	cout &lt;&lt; sizeof(a);
}
int main(){
	char a[] = "abcdef";
	char *b = "abcdef";
	size1(a); //  =&gt; sizeof(char*)
	size1(b); //  =&gt; sizeof(char*)
	size2(a); //  =&gt; sizeof(char*)
	size2(b);//  =&gt; sizeof(char*)
	cout &lt;&lt; sizeof(a)&lt;&lt;endl; // =&gt;strlen(a)+1
	cout &lt;&lt; sizeof(b)&lt;&lt;endl; // sizeof(char*)

}{% endcodeblock %}



{% codeblock %}// There are 5 segments in codes, DATA, BSS, TEXT, heap, stack
// DATA stores all globel variables, and those variables are defined.
// BSS stores all globel variables, but those variables are not defined.
// const variables are in TEXT, and codes are in this segment also.
// heap store (malloc, calloc,realloc, new) data
// stack store function params, local variables.

int a; // a is in BSS
int b = 3; // b is in DATA
int fun(int c){ // c is in stack, when the function is called.
	return 0; 
}
int main(){
	const int c = 4; // c is in TEXT
	a = 4; // move to DATA
	int x = 4; // in stack
	int * p = malloc(sizeof(int)); // *p is in heap. but p is in stack.
	return 0;
}{% endcodeblock %}
 
