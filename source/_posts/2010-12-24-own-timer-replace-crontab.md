---
layout: post
title: "own timer replace crontab"
date: 2010-12-24
wordpress_id: 346
comments: true
tags: ["cc", "code", "code", "crontab", "environment", "execute", "not", "program", "timer"]
categories:
- code
- program
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="crontab can not execute some program.
Its environment setting is hard to compare with that of system " />
<meta name="_su_keywords" content="crontab,execute,environment,timer,code,setting,profile" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="own timer replace crontab" />
<meta name="views" content="460" />
Sometime crontab in Linux or Unix can not run some program. Because the environment of crontab is not as same as that of system. If you want to know the difference between those two environments settings. look up /etc/cron.d/anacron file and ~/.profile and ~/.bashrc and xxxx. I can not remember all of them. /etc/cron.d/anacron file is just for crontab. ~/.profile and ~/.bashrc is for system. If you want to know more, search "linux environment file" in Google.

I think to find all difference between two environment is waste time obviously. Another method is write own timer codes. So Here is my solution:


{% codeblock %}#include &lt;iostream&gt;
#include &lt;string&gt;
#include &lt;stdlib.h&gt;
using namespace std;
/**
 * @author chico chen chillycreator #@@&amp; gmail.com
 */
void usage(){
	cout &lt;&lt; "timer"&lt;&lt;endl;
	cout &lt;&lt; "input your time point:[minute] [hour] [innerday] [innerweek]"&lt;&lt;endl;
	cout &lt;&lt; "input your program name:[your program]"&lt;&lt;endl;
	cout &lt;&lt; "all of input should be more than zero!"&lt;&lt;endl;
	cout &lt;&lt; "you should run this program earlier than you wanted."&lt;&lt;endl;
	cout &lt;&lt; "or else your program will run at next time point."&lt;&lt;endl;
}

int main(){

	// user input
	int min, hour, howday, howweek;
	string program;
	cout&lt;&lt; "input [min] [hour] [innerdays] [innerweeks]"&lt;&lt;endl;
	cin &gt;&gt; min &gt;&gt; hour &gt;&gt; howday &gt;&gt; howweek;
	cout &lt;&lt;"input your program"&lt;&lt;endl;
	cin &gt;&gt; program;
	if (program.size() &lt;= 0 || min &lt; 0 || hour &lt; 0 || howday &lt; 0 || howweek &lt; 0){
		usage();
		return 1;
	}
	cout &lt;&lt; "your timer will run program at "&lt;&lt;hour&lt;&lt;":"&lt;&lt;min&lt;&lt;endl;
	cout &lt;&lt; "your program is "&lt;&lt;program&lt;&lt;endl;
	if (0 == howday || 1 == howday){
		howday = 1;
		cout &lt;&lt; "run this program in every day"&lt;&lt;endl;
	}
	else{
		cout &lt;&lt; "run this program at "&lt;&lt;howday &lt;&lt;" days intervals."&lt;&lt;endl;
	}
	if (0 == howweek){
		cout &lt;&lt; "run this program in every week"&lt;&lt;endl;
	}
	else{
		cout &lt;&lt; "run this program at "&lt;&lt;howweek &lt;&lt;" weeks intervals."&lt;&lt;endl;
	}
	// set sleep time and loop time
	int looptime = (howday-1)*3600*24 + howweek * 7 * 3600 * 24;
	int sleeptime = 0;
	bool bFirst = true;
	while(true){
		time_t now;
		time(&amp;now);
		struct tm * timeinfo = localtime(&amp;now);
		timeinfo-&gt;tm_hour = hour;
		timeinfo-&gt;tm_min = min;
		time_t futuretime = mktime(timeinfo);
		sleeptime = futuretime - now;
		sleeptime = sleeptime &gt;= 0 ? sleeptime : 24*3600+sleeptime;
		if (!bFirst){
			sleeptime += looptime;
		}
		bFirst = false;
		cout &lt;&lt; "wait time:"&lt;&lt;sleeptime&lt;&lt;endl;
		sleep(sleeptime);
		cout &lt;&lt; "start to run:"&lt;&lt;program&lt;&lt;endl;
		system(program.c_str());
		cout &lt;&lt; "end program..."&lt;&lt;endl;
	}
	// loop end
	return 0;
}{% endcodeblock %}


Write your own code will bring new bugs in your solution. Another drawback is spending your own time and this timer is not in common. However, you can control it!
