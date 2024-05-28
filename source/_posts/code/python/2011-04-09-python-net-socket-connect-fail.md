---
layout: post
title: "python net socket connect fail"
date: 2011-4-9
wordpress_id: 389
comments: true
tags: ["connect", "error", "python", "python", "see-hear", "socket"]
categories:
- [code,python]
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_description" content="use python socket to connect server, but error occurs" />
<meta name="_su_keywords" content="python, socket, connect, error" />
<meta name="_su_title" content="python, socket, connect, error" />
<meta name="views" content="374" />
socket connect fail will be many reasons.
First you should check the firewall, does it block your socket port which you want to connect, python.exe or pythonw.exe
Second, your server listens host and ip are the same your client socket.
for example.
Your server socket listens host='localhost', port=7777, and your client want to connect host='192.168.12.XXX', port=7777. They are not same. 'localhost' is '127.0.0.1', and your client want to connect host='192.168.12.XXX'. Even there is only one net-adapter and one network configure. So change host which your server socket listened to '192.168.12.XXX'. 
This error will occur in :

```python
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('192.168.12.XXX',7777)) # here you can not connect server

```



