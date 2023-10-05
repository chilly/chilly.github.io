---
layout: post
title: "try...catch...exception in c++"
date: 2010-6-30
wordpress_id: 230
comments: true
tags: ["c", "cc", "catch", "exception", "throw", "try"]
categories:
- code

---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="try...catch...exception in c++, and it describe how to use it.  the philosophy of exceptions." />
<meta name="_su_keywords" content="c,c++,exception,throw, catch,try" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="try...catch...exception in c++" />
<meta name="views" content="2645" />
if you will throw something, you should catch it as the same type. like following:


{% codeblock %}int main(){
    try{
        throw 20;
    }catch(int i){
        cout &lt;&lt; "Exception Num is NO."&lt;&lt;i&lt;&lt;endl;
    }
    try{
        throw 'X';
    }catch(char c){
        cout &lt;&lt; "this exception is a char:"&lt;&lt;c&lt;&lt;endl;
    }

    try{
        throw "abc";
    }catch(const char * e){
        cout &lt;&lt; "this is string exception:"&lt;&lt;e&lt;&lt;endl;
    }

    try{
        int *d = new int(65);
        throw d;
    }catch(const int * e){
        cout &lt;&lt; "this is int point exception:"&lt;&lt;*e&lt;&lt;endl;
    }
    /* ok, catch will handle everything, if you throw correct type.*/
}{% endcodeblock %}


and if you want create classes for exceptions. You should add #include&lt;exception&gt; and inherit exception class, and then override the function "<strong>what</strong>".


{% codeblock %}#include&lt;iostream&gt;
#include&lt;exception&gt;
using namespace std;

/// this exception class is from #include&lt;exception&gt;
/// you should override the function what() to generate your exception string
class MyException: public exception{
    virtual const char* what() const throw(){
        return "myExceptions!!!!!!!";

    }

};

class SubException: public MyException{
    virtual const char* what()const throw(){
        return "sub exception !!!!!!";
    }

};

int main(){
    try{
        throw MyException();
    }catch(exception&amp; e){
        cout &lt;&lt; e.what()&lt;&lt;endl;

    }
    try{
        throw SubException();
    }catch(bad_alloc&amp;){
        // this will not occur, just test this type.
        // if it is occured, your computer will be locked.
        cout &lt;&lt; "nothing"&lt;&lt;endl;
    }catch(exception&amp; e){
        cout &lt;&lt; e.what()&lt;&lt;endl;

    }

}{% endcodeblock %}


There are some standard exceptions in exception.h. These exceptions are:

<strong>bad_alloc</strong>

A bad_alloc is thrown by new if an allocation failure occurs.

<strong>bad_cast</strong>

A bad_cast is thrown by dynamic_cast when it fails with a referenced  type.

<strong>bad_exception</strong>

A bad_exception is thrown when an exception type doesn’t match any  catch

<strong>bad_typeid</strong>

A bad_typeid is thrown by typeid

<strong>ios_base::failure</strong>

An ios_base::failure is thrown by functions in the iostream library.

in #include&lt;stdexecpt&gt; can use these exception:
class <strong>logic_error</strong>; // :  public exception
class <strong>domain_error</strong>; // :  public logic_error
class <strong>invalid_argument</strong>; // : public logic_error
class<strong> length_error</strong>; // :  public logic_error
class <strong>out_of_range</strong>; // :  public logic_error
class <strong>runtime_error</strong>; // :  public exception
class <strong>range_error</strong>; // :  public runtime_error
class <strong>overflow_error</strong>; // :  public runtime_error
class <strong>underflow_error</strong>; // :  public runtime_error

If you don't know which kind of exception will occur. you should use <strong>catch(...)</strong>


{% codeblock %}try{
        srand(time(NULL));
        int i = rand()%4;

        switch(i){
            case 0:
                throw; // terminate called without an active exception
                       // Aborted, ... will not catch it.
                break;
            case 1:
                throw 'x';
                break;
            case 2:
                throw 50;
                break;
            case 3:
                throw "my string";
                break;
            default:
                break;

        }

    }catch(...){
        cout &lt;&lt; "I don't know which exception will occur!"&lt;&lt;endl;
    }{% endcodeblock %}


But when i == 0, the program will execute throw;, and it will exit and print:
<blockquote>terminate called without an active exception

Aborted</blockquote>
<strong>You should specify something to throw. throw; is only be used within catch clauses.</strong>


{% codeblock %}try{
        try{
            throw 2;
        }catch(int i){
            throw; // here I use it again!
        }
    }catch(...){
        cout &lt;&lt; "re-throw it!!!!"&lt;&lt;endl;
    }{% endcodeblock %}


you can declare your functions using throw(). But it is just declare!  you can throw everything! But you declare it will throw bad_exception, and use set_unexpected(), it will process some unexpected exceptions.


{% codeblock %}void foo() throw(int,bad_exception){

    throw "abc";
}
void myunexpected(){
    cout &lt;&lt; "I get unexpected exceptions!"&lt;&lt;endl;
    throw;
}

// in main

    set_unexpected(myunexpected);
    try{
        foo();
    }catch(int){
        cout &lt;&lt; "catch a integer"&lt;&lt;endl;
    }catch(bad_exception&amp;){
        cout &lt;&lt; "bad_exception"&lt;&lt;endl;
    }
    // you can use catch(...){% endcodeblock %}


output is
<blockquote>I get unexpected exceptions!
bad_exception</blockquote>
If the program or your function can not process some kind of situation. you should throw something out.  Sometime, someone argued that it will reduce the usability. But If you don't stop the processing(program or function), it will create a disaster.  Remember, by all means, throw it! But should we catch those exceptions and how we catch those? e....as your wish. Some bug will not appear, if you use not appropriate catch and process those exceptions in unsuitable way. If your program have some bug, and it is difficult to find, please delete all catches!

In some languages which have finally clauses. But in c++, it dose not have finally clauses.
