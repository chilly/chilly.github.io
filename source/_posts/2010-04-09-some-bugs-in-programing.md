---
layout: post
title: "some bugs in programing"
date: 2010-4-9
wordpress_id: 146
comments: true
tags: ["bug", "c", "cc", "code", "define", "freopen", "macro", "math", "programming", "redirect"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="views" content="459" />
when you use double type to count, it will be dangerous. see like that

[codesyntax lang="c" tab_width="2"]
{% codeblock %}#include&lt;stdio.h&gt;
int main(){
  double i;
  for(i=0; i != 5; i++){
    printf("%lf ",i);
  }
  return 0;
}{% endcodeblock %}


it will never stop, because the condition of i != 5 will never satisfied. If you want to count, using int type. When you compare two double type number, you should use fabs(a-b) &lt; 0.001, 0.001 is threshold. Sometimes, double is not precision. macro is a great tool, but it is also dangerous. I often forget the language of define. The correct language is :

#define

#if defined

#if !defined

#ifndef

#ifdef

#endif

compile .c file with math lib in linux. It will be like that: cc test.c -o test <span style="color: rgb(255, 102, 0);">-lm</span>

stdio redirect to file use freopen() function. like that:


{% codeblock %}#include&lt;stdio.h&gt;
// don't write freopen here, it will be some compile error.
//freopen("test.in","r",stdin);
//freopen("test.out","w",stdout);
int main(){
	int n,i;
	#if defined REDIRECT
	freopen("test.in","r",stdin);
	freopen("test.out","w",stdout);
	#endif
	scanf("%d",&amp;n);
	for(i = 0; i &lt; n; i++){
		printf("%d ",i);
	}
	return 0;
}{% endcodeblock %}


if you want redirect to file, use the code and compile it with -D REDIRECT. cc test.c -o test -D REDIRECT

If you want to use struct FILE to do the same thing:

[codesyntax lang="c" tab_width="2"]
{% codeblock %}#include&lt;stdio.h&gt;
#include&lt;math.h&gt;
int main(){
  FILE * fin, *fout;
  fin = fopen("z.in","rb");
  fout = fopen("z.out","wb");
  int n;
  #ifdef REDIRECT
  #define fin stdin
  #define fout stdout
  #endif

  fscanf(fin,"%d",&amp;n);
  fprintf(fout,"%d,%lf",n,sqrt(n));
  fclose(fin);
  fclose(fout);
  return 0;
}{% endcodeblock %}


try cc t3.c -o t3 -lm -D REDIRECT and cc t3.c -o t3 -lm
