---
layout: post
title: "memory allocation in c"
date: 2010-7-5
wordpress_id: 238
comments: true
tags: ["allocation", "cc", "calloc", "code", "code", "free", "malloc", "memory", "realloc"]
categories:
- code
- program
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_description" content="c language , memory allocation about malloc function , calloc function and realloc function, free function and new and delete. " />
<meta name="views" content="1100" />
<meta name="_su_title" content="memory allocation in c" />
<meta name="_su_keywords" content="memory, allocation,malloc,calloc,realloc,free,new,delete,c,c++" />
In c language, there are three methods for memory allocation: malloc(), calloc(), realloc(). And one method to free memory: free().

The <strong>malloc</strong> function prototype is :<strong> void* malloc(size_t size);</strong> This function will allocates a block of <em>size</em> bytes of memory, returning a pointer to  the beginning of the block. <span style="color: #ff0000;">Attention:</span> this function is only for allocating memory! So the initial value of these memory will be Non-Zero. Maybe sometimes, it will be zero. Code for example:


{% codeblock %}#include&lt;stdio.h&gt;
#include&lt;stdlib.h&gt;
int main(){

	int * a = NULL;
	a = malloc(sizeof(int)*4);
	// like int a[4], but those memory is in heap
	for(int i = 0; i&lt; 4; i++)
		printf("%d ",a[i]);
	return 0;
}{% endcodeblock %}


The <strong>realloc</strong> function prototype is :<strong> void* realloc(void* ptr, size_t size);</strong> This function will reallocates a block of <em>size</em> bytes of memory,  returning a pointer to  the beginning of the block. If size is zero and ptr is not <strong>NULL</strong>, it will be used as <strong>free()</strong>. If prt is NULL and size is not zero, it will be used as <strong>malloc()</strong>. If ptr is NULL and size is zero, return value is some kind of memory which contains zero elements! Wooo~~~~. Look at the following codes:


{% codeblock %}char * p = NULL;
printf("%x",(int)p); // will output 0

p = (char*)realloc(p,0);
printf("%x",(int)p); // will output non-zero

p = (char*)realloc(p,0);
printf("%x",(int)p);{% endcodeblock %}


why?! Because if this function is called successfully, it will return non-zero memory address. And you will see the third output <strong>subtract </strong>the second output will be<strong> 16 bytes</strong>. If ptr is not NULL , and size is non-zero, and current block can simply expand without moving,  the return value is the same as ptr. But there is not enough space for simply expand, it will execute<strong> free(ptr) and return malloc(size)</strong>. This function will become very <span style="color: #ff0000;">dangerous!</span>


{% codeblock %}char * k = (char*)realloc(NULL,100);
char * kbigger = (char*)realloc(k,104);
char * kbiggest = (char*)realloc(k,106); <span style="color: #ff0000;">// crash!</span>{% endcodeblock %}


Safety called will be like the following:


{% codeblock %}char * k = (char*)realloc(NULL,100);
k = (char*)realloc(k,4); // k's address is not changed
k = (char*)realloc(k,104); // k's address is changed{% endcodeblock %}


If there is not enough free memory to be allocated, this function will return NULL.<span style="color: #ff0000;">Attention:</span> this function will not initialize those memory also.

<strong>calloc</strong>'s prototype is void* calloc(int num, size_t size).


{% codeblock %}char* p = (char*)calloc(20,sizeof(char));

// it is equal to

char* p = (char*)malloc(20*sizeof(char));
memset(p,0,20*sizeof(char));{% endcodeblock %}


<span style="color: #ff0000;">But calloc is efficient!</span>

<span style="color: #ff0000;"> <strong> </strong><span style="color: #000000;"><strong>free</strong> function is to delete memory which has been allocated. But free function will not assign the point to zero.</span></span>

<span style="color: #ff0000;"><span style="color: #000000;"></span></span>
{% codeblock %}char * p = (char*)calloc(100);

p[0] = 'a';
printf("%c",p[0]);   // output 'a'
printf("%x",(int)p);
free(p);
printf("%c",p[0]); // output nothing
printf("%x",(int)p); // the value is the same as the first.{% endcodeblock %}


And all of those functions will not recommend to be used in c++, because those functions will not initialize class structure or v_ptr. free() function will not destruct parent class when sub class is destructed. In c++, maybe should always use <strong>new</strong> and <strong>delete</strong>!
