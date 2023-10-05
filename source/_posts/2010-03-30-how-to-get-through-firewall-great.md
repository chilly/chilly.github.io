---
layout: post
title: "how to get through firewall (Great)"
date: 2010-3-30
wordpress_id: 132
comments: true
tags: ["ack", "connection", "firewall", "open-source", "see-hear"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="views" content="631" />
This is graph for getting through firewall.  This is another method for climbing over the wall.

Phase1. setup the connection between client and server by three-way handshake. when server send ACK and seq to the client. Client should let the wall know this connection is over. So client send FIN and seq to server. The wall get it, and consider the connection from client is over. Because the wall is not based on status. Then server get it, and find the seq of  package is wrong. And server send the RESET package to client. The wall get the package and consider the connection from server is over. Client receive and ignore the RESET package, and then it send the ACK and seq+1 to server.  The connection between client and server is established. However, the connection between client and server is over from viewpiont of the wall. The wall will not exam contents of the connection. So we can get though the wall!

<!--more-->Here is the graph to show how to get through firewall.

<a href="http://chillyc.info/wp-content/uploads/2010/03/30/how-to-get-through-firewall-great/large_zhang.png"><img class="aligncenter size-medium wp-image-135" title="large_zhang" src="http://chillyc.info/wp-content/uploads/2010/03/30/how-to-get-through-firewall-great/large_zhang-300x225.png" alt="" width="300" height="225" /></a>
