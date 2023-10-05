---
layout: post
title: "quicksort in linux or Unix"
date: 2010-4-19
wordpress_id: 182
comments: true
tags: ["asce", "cc", "code", "linux", "qsort", "quicksort", "sort", "stable", "unix"]
categories:
- code
- program
---
<meta name="views" content="1142" />
<meta name="_edit_last" content="1" />
qsort is quicksort function in linux or Unix. The function prototype is


{% codeblock %}void qsort(const void* array, const int number, const size_t sizeof_element,int (*cmp_fun)(const void*,const void*));{% endcodeblock %}


maybe I write too many const here:) I will write some examples to sort numbers below:


{% codeblock %}#include&lt;stdlib.h&gt;
// in c++ use cstdlib
int cmp(const void * a, const void * b){
	const int *tmp_a = (const int*)a;
	const int *tmp_b = (const int *)b;
	return *tmp_a-*tmp_b; // this is ASCE
} // this is function for comparing two Int type

int main(){
	int array[100],i,flag = 0;
	for(i =0; i &lt;100; i++){
		scanf("%d",&amp;array[i]);	
	}
	qsort(array,100, sizeof(array[0]), cmp);
	// here you can use sizeof(int) instead
	for(i = 0; i &lt; 100; i++){
		if(!flag){
			printf("%d", array[i]);
			flag = 1;
		}
		else{
			printf(" %d", array[i]);
		}
	}
	printf("\n");
	return 0;
}{% endcodeblock %}


qsort function will change the content of array. If you want use complex object in qsort. You should just convert const void* to object*. If you will use point in array, you should compare with objects use **tmp_a-**tmp_b. If you want to compare double or float type, you should have a look at my another article: <a title="Permanent Link: compare double numbers in c/c++ language" rel="bookmark" href="../archives/172">compare  double numbers in c/c++ language.</a>

If you want a stable compare function, you can use qsort also. First you should construct a class or struct, then and index or other information into struct. And compare when two values are equal.


{% codeblock %}int cmp(const void * a, const void * b){
	const Object *tmp_a = (const Object*)a;
	const Object *tmp_b = (const Object *)b;
	if(tmp_a-&gt;value == tmp_b-&gt;value){
		if(tmp_a-&gt;index &lt; tmp_b-&gt;index){
			return -1;
		}
		else{
			return 1;
		}
	}
	return tmp_a-&gt;value-tmp_b-&gt;value; // this is ASCE
} // this is function for comparing two Int type{% endcodeblock %}
 
