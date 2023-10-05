---
layout: post
title: "how to debug PostgreSQL"
date: 2010-5-10
wordpress_id: 208
comments: true
tags: ["code", "debug", "postgresql", "postgresql", "symbol"]
categories:
- code
- program
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="postgresql debug" />
<meta name="_su_keywords" content="postgresql debug" />
<meta name="_su_title" content="postgresql debug" />
<meta name="views" content="1123" />
First, compile the source code use ./configure --prefix=XXXX --enable-debug &amp;&amp; CFLAGS=-O0

then make and make install.
<h2><span style="color: #800000;">IF you want to RESTART PG</span></h2>
Detected if there is an instance of Postgre server. If there is, kill it. use


{% codeblock %}ps -e | grep postgre
// find id of the process
kill -9 $processid{% endcodeblock %}


to detect the pocess of postgresql and kill it.

then use:


{% codeblock %}gdb postgresqlHomeDIR/bin/postgres -quiet
// enter gdb then use
set args -D DATADIR
run{% endcodeblock %}


here DATADIR and postgresqlDIR are your own directory.
<h2><span style="color: #800000;">IF you don't want to RESTART PG</span></h2>

{% codeblock %}// find postgres pid
pid = `ps auxww | \
    grep postgres: | \
    grep -v -e 'grep postgres:' -e 'postgres: stats' -e 'postgres: writer' -e 'postgres: archiver' -e 'postgres: logger' -e 'postgres: autovacuum' | \
    tee /dev/tty | \
    awk '{print $2}'`
gdb pid{% endcodeblock %}


then use


{% codeblock %}./psql test{% endcodeblock %}


to test. Here test is  your database name.

IF you want to debug contrib

make &amp;&amp; make install in contrib/yourdir

then go to gdb to file your shard lib to load symbol. Here XXX.so is your contrib share lib.


{% codeblock %}(gdb) file XXX.so{% endcodeblock %}


if not, when you use break somefunction, it will report:

No symbol table is loaded.  Use the "file" command.
Make breakpoint pending on future shared library load? (y or [n])
