---
layout: post
title: "Build SQLite in Code::blocks"
date: 2010-4-22
wordpress_id: 185
comments: true
tags: ["blocks", "build", "cc", "code", "database", "error", "open-source", "project", "readline", "sqlite", "sqlite3api", "tcl"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="views" content="1356" />
first download <a href="http://www.sqlite.org/sqlite-3.6.23.1.tar.gz">a source tarball</a>, then add all *.h and *.c into code::blocks. and remove tclsqlite.c file. Then copy tcl.h, tclDecls.h, and tclPlatDecls.h from /usr/include/tclXXX/ to your project forld and add them into your project. Then choose your project name in the left Projects browser. and right click, then choose build options. like following:

<a href="http://chillyc.info/wp-content/uploads/2010/04/22/install-sqlite-in-codeblocks/Screenshot-fts3_tokenizer.c-SQLite-CodeBlocks-8.02.png"><img class="aligncenter size-medium wp-image-186" title="Screenshot-fts3_tokenizer.c [SQLite] - Code::Blocks 8.02" src="http://chillyc.info/wp-content/uploads/2010/04/22/install-sqlite-in-codeblocks/Screenshot-fts3_tokenizer.c-SQLite-CodeBlocks-8.02-300x218.png" alt="" width="300" height="218" /></a>In the opening window, choose other option tab and add some options:

<a href="http://chillyc.info/wp-content/uploads/2010/04/22/install-sqlite-in-codeblocks/Screenshot-Project-build-options-1.png"><img class="aligncenter size-medium wp-image-187" title="Screenshot-Project build options-1" src="http://chillyc.info/wp-content/uploads/2010/04/22/install-sqlite-in-codeblocks/Screenshot-Project-build-options-1-300x218.png" alt="" width="300" height="218" /></a>then choose #define tab:

<a href="http://chillyc.info/wp-content/uploads/2010/04/22/install-sqlite-in-codeblocks/Screenshot-Project-build-options.png"><img class="aligncenter size-medium wp-image-188" title="Screenshot-Project build options" src="http://chillyc.info/wp-content/uploads/2010/04/22/install-sqlite-in-codeblocks/Screenshot-Project-build-options-300x218.png" alt="" width="300" height="218" /></a>ok, done. go back and build the project. If you have sqlite3api multiply defined error, you may don't use SQLITE_CORE macro. If you have realine function dose not have prototype, you may don't have "libreadline", or you not use -lreadline and -lcurses option links. If the compiler tells you:"can not find "tcl.h" head file", you can copy tcl.h, tclDecls.h, and tclPlatDecls.h into your project, or change tcl.h to &lt;tcl.h&gt;. If you have some error with tcl, you may leave the tclsqlit.c file from building. If you persist in compile tclsqlit.c, you should merge its all codes into main.c.
