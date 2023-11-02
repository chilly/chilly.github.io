---
layout: post
title: "postgresql function define"
date: 2010-9-3
wordpress_id: 321
comments: true
tags: ["cc", "code", "database", "define", "function", "own", "postgresql", "postgresql", "self", "type"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="views" content="1573" />
In Postgresql, If you want to do something like "select Hello(a) from A;". You should define your own function Hello().

First, go to postgresql root directory and go into contrib file folder. Create your own directory, and add hello.h , hello.c, Makefile, hello.sql.in and uninstall_Hello.sql.in

Makefile is like the following:


{% codeblock %}MODULE_big  = Hello // this is module name
OBJS = Hello.o // those will be create by execute "make"

DATA_built = hello.sql // install your function using this sql file
DATA = uninstall_Hello.sql
SHLIB_LINK = $(BE_DLLLIBS)

ifdef USE_PGXS // this is a build infrastructure of extentions in postgreSQL
PGXS = $(shell pg_config --pgxs)
include $(PGXS)
else
subdir = contrib/Hello // here is your code files
top_builddir = ../..
include $(top_builddir)/src/Makefile.global
include $(top_srcdir)/contrib/contrib-global.mk
endif

// the following code fragment is copying content of *.sql.in into *.sql
%.sql: %.source
	rm -f $@; \
	C=`pwd`; \
	sed -e "s:_OBJWD_:$$C:g" &lt; $&lt; &gt; $@{% endcodeblock %}


Attention!!! In Makefile those command should be with no blank at end.
Here we can write hello.sql.in:


{% codeblock %}SET search_path = public; --you should add this code first.
--functions
--take care of those functions, if those functions are used for data input and out, their args or return type must be cstring or varchar

-- arg type is cstring
CREATE OR REPLACE FUNCTION hello_in(cstring)
   RETURNS HelloS
   AS 'MODULE_PATHNAME'
   LANGUAGE C IMMUTABLE STRICT;
-- return type is cstring
CREATE OR REPLACE FUNCTION hello_out(HelloS)
   RETURNS cstring
   AS 'MODULE_PATHNAME'
   LANGUAGE C IMMUTABLE STRICT;

--data type
CREATE TYPE HelloS (
   internallength = 52,
   input = hello_in,
   output = hello_out,
   alignment = double
);
--size is 52 bytes and alignment is 8 bytes.
--if you don't know its real size, use internallength = VARIABLE
--if you want use binary array as its input and out, you should use receive=XXX_function, and send = XXX_function

--outer function
--this function can use kinds of types
CREATE OR REPLACE FUNCTION Hello(HelloS)
   RETURNS cstring
   AS 'MODULE_PATHNAME'
   LANGUAGE C IMMUTABLE STRICT;{% endcodeblock %}

Here is the uninstall_Hello.sql.in:


{% codeblock %}SET search_path = public;

DROP FUNCTION hello_in(cstring);
DROP FUNCTION hello_out(HelloS);
DROP TYPE HelloS;
DROP FUNCTION Hello(HelloS);{% endcodeblock %}


In hello.h


{% codeblock %}#ifndef HELLO_H
#define HELLO_H
#include&lt;stdio.h&gt;
#include&lt;stdlib.h&gt;

#include "postgres.h"
#include "fmgr.h"
#include "funcapi.h"
// you should take care of the order of those head files!!!

typedef struct{
	char content[52];
}HelloS;

Datum		hello_in(PG_FUNCTION_ARGS);
Datum		hello_out(PG_FUNCTION_ARGS);
Datum               Hello(PG_FUNCTION_ARGS);

#endif{% endcodeblock %}


hello.c


{% codeblock %}#include “hello.h”

#ifndef MOD_FOR_PG
PG_MODULE_MAGIC;
#endif

PG_FUNCTION_INFO_V1(hello_in);
Datum
hello_in(PG_FUNCTION_ARGS)
{
	// get args[0]
	char * str = PG_GETARG_CSTRING(0);
    HelloS * hs = palloc(sizeof(HelloS));
    if(NULL != str){
        strcpy(hs-&gt;content,str);
        PG_RETURN_POINTER(hs)
    }
    PG_RETURN_NULL();
}{% endcodeblock %}
{% codeblock %}PG_FUNCTION_INFO_V1(hello_out);
Datum
hello_out(PG_FUNCTION_ARGS)
{
	// get args[0]
    Hello * hs = PG_GETARG_POINTER(0);

    if(NULL != hs){
        char * content = palloc(sizeof(HelloS));
        strcpy(content,hs-&gt;content);
        PG_RETURN_CSTRING(content);
    }
    PG_RETURN_CSTRING("");
}

PG_FUNCTION_INFO_V1(Hello);
Datum
Hello(PG_FUNCTION_ARGS)
{
   HelloS* hs = PG_GETARG_POINTER(0);
 if(NULL != hs){
    char * res = palloc(sizeof(HelloS));
    sprintf(res,"hello %s",hs-&gt;content);
    PG_RETURN_CSTRING(res);
 }
 PG_RETURN_CSTRING("hello nobody");
}{% endcodeblock %}


Then cd your folder contrib/Hello, make, and make install

Then start psql to execute :

create table A(a HelloS);

insert into A values('cc');

insert into A values('ff');

select Hello(a) from A;

result:

Hello cc

Hello ff

select * from A;

result:

cc

ff
