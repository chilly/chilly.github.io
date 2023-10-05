---
layout: post
title: "use doxygen"
date: 2010-4-16
wordpress_id: 176
comments: true
tags: ["auto", "code", "document", "doxygen", "generate", "how-to", "open-source", "project", "see-hear"]
categories:
- code
- program
---
<meta name="views" content="677" />
<meta name="_edit_last" content="1" />
<span><span style="font-family: Verdana,Arial,Helvetica;"><a id="step2">doxygen is  a tool to generate documents. So you don't worry about writing mountainous documents of codes.  It can generate pdf, html, latex and also doc. But it will abstract the comment and keyword to generate documents. If your comments is not suitable for doxygen, it  will generate into documents.  First , get doxygen tool from Internet or use "<strong>sudo apt-get install doxygen</strong>" from ubuntu. Then cd to your project root, and type: "<strong>doxygen -g</strong>" to the configure file of generating document. Open New "Doxyfile" with vim or gedit. There are some labels in this configure file. label format is:
</a></span></span>

# All text after a hash (#) is considered a comment and will be ignored
# The format is:
#       <strong>TAG = value [value, ...]</strong>
# For lists items can also be appended using:
#       <strong>TAG += value [value, ...]</strong>
# Values that contain spaces should be placed between quotes (" ")

There are some useful label for cpp or c project.

# this label will let you to use /// or //! comments to generate documents.

<strong>MULTILINE_CPP_IS_BRIEF = YES</strong>

# what's the size of tab in your code documents?

<strong>TAB_SIZE               = 4</strong>

# what's the name of project?

<strong>PROJECT_NAME           = CNODE</strong>

# If you use chinese or some other non-English language...

<strong>DOXYFILE_ENCODING      = UTF-8</strong>

# which patter you want to generate documents? If you left blank, doxygen will parse to kinds of files. But here, I just use two kinds files.

<strong>FILE_PATTERNS          = *.h
FILE_PATTERNS          += *.c</strong>

# will you want recursive the code paths?

<strong>RECURSIVE              = YES</strong>

Save "Doxyfile" and run command: "<strong>doxygen</strong>"<strong>
</strong>
