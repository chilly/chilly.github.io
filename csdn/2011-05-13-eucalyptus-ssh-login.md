---
layout: post
title: "eucalyptus ssh can not login solution"
date: 2011-5-13
wordpress_id: 413
comments: true
tags: ["eucalyptus", "login", "open-source", "project", "solution", "ssh"]
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="views" content="274" />
I used
<blockquote>euca-describle-keypairs</blockquote>
to see the password first. If there is no password "aa",
then use the following command.
<blockquote>euca-add-keypair aa > my.private</blockquote>
else delete password "aa" first or use another password.
<blockquote>euca-delete-keypair aa</blockquote>
you should change my.private file access privilege, like:
<blockquote>chmod 0600 my.private</blockquote>
then use euca-run-instance to run:
<blockquote>euca-run-instance -k aa -n 1 $emi</blockquote>
then I have seen ip of instance.
use ssh to login:
<blockquote>ssh -i my.private root@ip</blockquote>
done!

If you do not change my.private file access privilege, then it will report:
<blockquote>privilege 0644 is too open.</blockquote>
If you chmod after the report, ssh will not let you login. So change it before you want access your instance.
