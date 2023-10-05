---
layout: post
title: "What does volatile do?"
date: 2010-6-21
wordpress_id: 224
comments: true
tags: ["cc", "code", "synchronizes", "volatile"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="views" content="536" />
<meta name="_wp_old_slug" content="copy" />
copy from http://www.javaperformancetuning.com/news/qotm030.shtml

<strong>What does {% codeblock %}volatile{% endcodeblock %} do?</strong>

This is probably best explained by comparing the effects that {% codeblock %}volatile{% endcodeblock %} and {% codeblock %}synchronized{% endcodeblock %} have on a method. {% codeblock %}volatile{% endcodeblock %} is a  field modifier, while {% codeblock %}synchronized{% endcodeblock %} modifies code blocks and  methods. So we can specify three variations of a simple accessor using those two  keywords:
{% codeblock %}         int i1;              int geti1() {return i1;}
volatile int i2;              int geti2() {return i2;}
         int i3; synchronized int geti3() {return i3;}
{% endcodeblock %}
{% codeblock %}geti1(){% endcodeblock %} accesses the value currently stored in {% codeblock %}i1{% endcodeblock %} <em>in the current thread</em>. Threads can have local copies of variables, and  the data does not have to be the same as the data held in other threads. In  particular, another thread may have updated {% codeblock %}i1{% endcodeblock %} in it's thread, but the  value in the current thread could be different from that updated value. In  fact Java has the idea of a "main" memory, and this is the memory that holds the  current "correct" value for variables. Threads can have their own copy of data  for variables, and the thread copy can be different from the "main" memory.  So in fact, it is possible for the "main" memory  to have a value of 1 for {% codeblock %}i1{% endcodeblock %}, for thread1 to have a value of 2 for {% codeblock %}i1{% endcodeblock %} and for thread2 to  have a value of 3 for {% codeblock %}i1{% endcodeblock %} if thread1 and thread2 have both updated {% codeblock %}i1{% endcodeblock %} but those updated value has not yet been propagated to "main" memory or  other threads.

On the other hand, {% codeblock %}geti2(){% endcodeblock %} effectively accesses the value  of {% codeblock %}i2{% endcodeblock %} from "main" memory. A {% codeblock %}volatile{% endcodeblock %} variable is not allowed to  have a local copy of a variable that is different from the value currently held in "main"  memory. Effectively, a variable declared {% codeblock %}volatile{% endcodeblock %} must have it's data  synchronized across all threads, so that whenever you access or update the variable in any thread, all  other threads immediately see the same value. Of course, it is likely that {% codeblock %}volatile{% endcodeblock %} variables have a higher access and update overhead than "plain"  variables, since the reason threads can have their own copy of data is for better efficiency.

Well if {% codeblock %}volatile{% endcodeblock %} already synchronizes data across threads,  what is {% codeblock %}synchronized{% endcodeblock %} for? Well there are two differences. Firstly {% codeblock %}synchronized{% endcodeblock %} obtains and releases locks on monitors which  can force only one thread at a time to execute a code block, if both threads use  the same monitor (effectively the same object lock). That's the fairly well known  aspect to {% codeblock %}synchronized{% endcodeblock %}. But {% codeblock %}synchronized{% endcodeblock %} also  synchronizes memory. In fact {% codeblock %}synchronized{% endcodeblock %} synchronizes the whole of thread  memory with "main" memory. So executing {% codeblock %}geti3(){% endcodeblock %} does the following:
<ol>
	<li>The thread acquires the lock on the monitor for object {% codeblock %}this{% endcodeblock %} (assuming the monitor is unlocked, otherwise the thread waits until the  monitor is unlocked).</li>
	<li>The thread memory flushes <em>all</em> its variables, i.e. it  has all of its variables effectively read from "main" memory (JVMs can use dirty sets to optimize  this so that only "dirty" variables are flushed, but conceptually this is the same. See  section 17.9 of the Java language specification).</li>
	<li>The code block is executed (in this case setting the return  value to the current value of {% codeblock %}i3{% endcodeblock %}, which may have just been reset from "main"  memory).</li>
	<li>(Any changes to variables would normally now be written out to  "main" memory, but for {% codeblock %}geti3(){% endcodeblock %} we have no changes.)</li>
	<li>The thread releases the lock on the monitor for object {% codeblock %}this{% endcodeblock %}.</li>
</ol>
So where {% codeblock %}volatile{% endcodeblock %} only synchronizes the value of one  variable between thread memory and "main" memory, {% codeblock %}synchronized{% endcodeblock %} synchronizes  the value of all variables between thread memory and "main" memory, and locks and  releases a monitor to boot. Clearly {% codeblock %}synchronized{% endcodeblock %} is likely to have  more overhead than {% codeblock %}volatile{% endcodeblock %}.
