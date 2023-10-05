---
layout: post
title: "Software Architecture--Singleton"
date: 2010-4-5
wordpress_id: 144
comments: true
tags: ["architecture", "single", "singleton", "software-architecture-code", "software-architecture"]
categories:
- architect
---
<meta name="_edit_last" content="1" />
<meta name="views" content="530" />
Singleton is a pattern in Software Architecture. It is used to create a instance and there will be only one instance in system at any time. If you want to write this pattern in a file, it will be like that:

[codesyntax lang="cpp" tab_width="2"]
{% codeblock %}#include &lt;iostream&gt;
using namespace std;
/* ==========================
 * Program name:
 * Author: chico chen
 * Email:  chillycreator @t gmail.com
 * Purpose: this class is testing singleton pattern in
 *          Software Architecture.
 * ========================== */

class Singleton {
	private:
	static Singleton* _instance;
	Singleton(){}
	public:
		static Singleton* Instance();
};
Singleton* Singleton::_instance = NULL;
Singleton* Singleton::Instance() {
    if (_instance == NULL) {
        _instance = new Singleton();
        cout &lt;&lt;"new Singleton"&lt;&lt;endl;
    }
    return _instance;
}

int main() {
    cout &lt;&lt; "Hello world!" &lt;&lt; endl;
    Singleton *s1 = Singleton::Instance();
    Singleton *s2 = Singleton::Instance();
    Singleton *s3 = Singleton::Instance();
    Singleton *s4 = Singleton::Instance();
    return 0;
}{% endcodeblock %}


If you want write this pattern into different files, it will has Singleton.h Singleton.cpp and other files like that:

<!--more-->Singleton.h

[codesyntax lang="cpp" tab_width="2"]
{% codeblock %}/* ==========================
 * Program name:
 * Author: chico chen
 * Email:  chillycreator @t gmail.com
 * Purpose: this class is testing singleton pattern in
 *          Software Architecture.
 * ========================== */

class Singleton {
	private:
	static Singleton* _instance;
	Singleton();
	public:
		static Singleton* Instance();
};{% endcodeblock %}


Singleton.cpp

[codesyntax lang="cpp" tab_width="2"]
{% codeblock %}#include "Singleton.h"
Singleton::Singleton(){}
Singleton* Singleton::_instance = NULL;
Singleton* Singleton::Instance() {
    if (_instance == NULL) {
        _instance = new Singleton();
        cout&lt;&lt;"new Singleton"&lt;&lt;endl;
    }
    return _instance;
}{% endcodeblock %}


main.cpp

[codesyntax lang="cpp" tab_width="2"]
{% codeblock %}#include &lt;iostream&gt;
#include "Singleton.h"
using namespace std;
int main() {
    cout &lt;&lt; "Hello world!" &lt;&lt; endl;
    Singleton *s1 = Singleton::Instance();
    Singleton *s2 = Singleton::Instance();
    Singleton *s3 = Singleton::Instance();
    Singleton *s4 = Singleton::Instance();
    return 0;
}{% endcodeblock %}


But it dose not ensure this instance is only the same in the system. Because it will be destroyed and recreated.
