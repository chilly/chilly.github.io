---
layout: post
title: "design pattern CHAIN of reponsibility"
date: 2010-5-24
wordpress_id: 213
comments: true
tags: ["cc", "chain", "code", "design", "pattern", "reposibility", "software-architecture-code"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="design pattern CHAIN of reponsibility" />
<meta name="_su_keywords" content="design pattern CHAIN of reponsibility" />
<meta name="_su_title" content="design pattern CHAIN of reponsibility" />
<meta name="views" content="603" />

{% codeblock %}#include&lt;cstdio&gt;

using namespace std;
namespace pattern{
	/// this pattern links all handler as a line, if previous handler can process a request, it wil not be
	/// send to next handler. For example, if someone requested changing programming style, 
	/// this decision will be made by a programmer. 
	/// He will change his programming style without noticing his lead. 
	/// But if someone request changing the requirements, the programmer can not make this decision. 
	/// He will noticing his lead, and make his lead decided.

	/// this is request class.
	#define STYLE 0
	#define REQIREMENT 1
	class Request{
		public:
			int type; // changing programming style or changing requirements?

	};

	/// this class for processing request.
	class Handler{
	private:
		Handler* parent;
	public:
		Handler(Handler* p):parent(p){}
		virtual bool DoHandle(Request&amp; r)=0;
		void Process(Request&amp; r);
		virtual ~Handler(){}
	};

	void Handler::Process(Request&amp; r){
		if(!this-&gt;DoHandle(r) &amp;&amp; NULL != parent){
			parent-&gt;Process(r);
		}
	}

	class Programmer: public Handler{
		public:
		Programmer(Handler* p):Handler(p){}
		virtual bool DoHandle(Request&amp; r);

	};
	bool Programmer::DoHandle(Request&amp; r){
		if(r.type == STYLE){
			printf("programmer: I can process changing programming style\n");
			return true;
		}else{
			printf("programmer: I can not process this request. I will report this to my lead!\n");
			return false;
		}

	}

	class Lead:public Handler{
		public:
		Lead(Handler* p):Handler(p){}
		virtual bool DoHandle(Request&amp; r);

	};
	bool Lead::DoHandle(Request&amp; r){
		if(r.type == REQIREMENT){
			printf("lead: I will process changing reqirements\n");
			return true;
		}
		return false;
	}
}

int main(){
	pattern::Lead lead((pattern::Handler*)NULL);
	pattern::Programmer programmer(&amp;lead);
	pattern::Request r;
	r.type = STYLE;
	programmer.Process(r);
	r.type = REQIREMENT;
	programmer.Process(r);
	return 0;

}{% endcodeblock %}
 
