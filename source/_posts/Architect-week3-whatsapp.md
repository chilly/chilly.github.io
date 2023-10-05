---
title: 'Architect:week3-whatsapp'
date: 2017-09-13 20:06:17
tags: ['Architect','whatsapp','leetcode','week3','architect','data model']
categories:
- architect
---
# Origin
I joined leetcode system design group to discuss how to design software. This is week3 topic:

**Messenger service like What's App** with the following functionalities

1. One to One text messaging
2. Group Chat, Storage
3. Read/Delivery/Sent status

Gist:

1. We are having a conversation between two parties. A -&gt; B. Think of this process. What happens and how do you explain to interviewer
2. Asynchronous - meaning, "You are on a flight and you receive messages once you land. So those messages were waiting some where!" (Hope you got it). How would you design it
3. Horizontal scaling, Load Balancer
4. For Read/Delivery/Sent status -&gt; 3 way handshake protocol can give you an idea.


This is a good topic. I will try to design it. 

# Scenario

First of all, what's the scenarios? The scenarios will correct my design. The scenario is in which way people use our software. So let's think scenario first. The scenario should be atomic, and can not split into two or more scenarios. The scenario is a completed interaction. For example, The scenario like:

1. User A send message to the system.
1. The system send message to User B.

Step 1 or Step 2 is not a scenario. Step 1 with Step 2 is a completed scenario.
And our chat system will be like:

<div align=center>
![init-system](/images/Architect-week3-whatsapp/init-system.png)
**Figure 2.1** init system
</div>

Er...It's too simple...So let's write more scenarios:

##  Scenario 1: send and get message in realtime

1. User A send message to the system.
1. The system send message to user B.

##  Scenario 2: send message and get it after a while, because user is offline.

1. User A send message to the system.
1. At this time, user B is offline.
1. User B is online.
1. The system send message to User B.

### Scenario 3: read old messages

1. User can read old messages of chat in receive-time order.

## Scenario 4: group chat in realtime

1. User A, B, and C join a chat group
1. A send message to the system
1. B and C receive the message from system.

## Scenario 5: group chat when someone is offline

1. User A, B, and C join a chat group
1. User A send message to the system.
1. C will receive the message in realtime.
1. At this time, user B is offline.
1. When User B is online, the system send message to User B in create-time order.


## Scenario 6: chat status changing

1. User A chat with B
1. When A is typing words, B will get "A is typing..." message from system.
1. When A press "send" button, A will get "the message is sending" from system.
1. When B's app receive the message in background, A will get "the message is delivered". 
1. When B open the chat with A, and scroll to the message, A will get "the message has been read."

Here, scenario 5 and 4 is similar to 2 and 1. So can we consider scenario 2 and 1 are group chat with only one person? Can I chat with myself? And there should be more scenarios be considered. For example:
1. User sign in
1. User sign up
1. User quit
1. Invite other user to use the app
1. Find a user
1. Add relationship with other user, family, friends, enemy, lover, couple, and so on. Others should comfire the relationship.
1. Break the relationship.
1. Rebuild the relationship with the same user. Er...Are you crazy? ...Trust me, most of lovers will repeat break-rebuild relationship scenarios. If you don't believe it, you must be younger than me.
1. Build a chat group, or invite other user one by one to join chat/group chat.
1. Exit a chat group, but can not exit one-one chat, unless breaking the relationship.
1. Type message with emoji, gif, png, voice, and other rich text.
1. Search old messages
1. Delete old messages.
1. Set/Show online/offline or other user status to one or more users.
1. Can we chat face to face?
1. Change user own info

In this article, we can not talk all above. It's too complicated. I will only focus on group chat and chat status changing scenarios. 

<div align=center>
![scenarios](/images/Architect-week3-whatsapp/scenarios.png)
**Figure 2.2** scenarios relationship
</div>

Like Figure 2.2 describles, one-one chat is a special case of group chat. So here use inherit arrow. The interaction of scenario 6 will be included from group chat scenarios. But scenario 3 is alone. If scenario 3 is discarded, our message service will be like "Snapchat". And I use "User" actor instead of "User A" or "User B". Why I always write a rectangle as "Messager service"? Because the rectangle warns us, our service has its own boundary! We will not consider other scenarios. If those scenarios inside of rectangle do not be implemented, our service is useless. And if you want to add more functionalities, the rectangle will be bigger and bigger. So thinking about your limit time and money, can you add more functionalities? The correct thing is creating worked well service, notwriting piles and piles of functionalities. Mmmmm...sound like: I use your "Messager service", will you add "beautify photo"? **NO!!!**

# How to design?

Seems like we always talk about how user use our messager service. Er...Correct! So here I will write something about how to design it. I don't know how to build the system. I only knows there should be a client and a server...So like that:

<div align=center>
![init-components](/images/Architect-week3-whatsapp/init-components.png)
**Figure 3.1** init components
</div>

Now, we have first components diagram. Let's do more thinking. If we store data, we will have database. If we connect with others, there should have a channel mananger. My chat groups should be controlled by group manager. If some user is offline, can we send offline message to them by APNS? Adding schedule component for sending message to offline user is a good idea. There should have a component for transforming "chat message" to service data, and doing reverse. And another component called "chat service" controls all component to work togather.

<div align=center>
![more-components](/images/Architect-week3-whatsapp/more-components.png)
**Figure 3.2** decompose components
</div>

In Figure 3.2, I decompose "Client" into "Interface", "Transform" and "Channel", and decompose "Server" into other small components. How to use line to connect those components? The answer is if the connection between components exists, connecting them. It's nosense!! And another way to deal with the connections is following data flows. In Message services, there exists at least two data flows.

1. User -> Client Interface -> Transform message to bytes/data -> send to Channel -> Server Channel Mgr receive bytes/data -> Transfrom into server data -> according to data, pull group data from Group Mgr and store message into Database.
1. According to group info, sending message to other group members ->  Transfrom server data into bytes -> send to Channel Mgr -> delivery message to Client Channel -> Transform bytes to message -> display message to User.
1. Schedule reading what cannot be deliveried -> send message through Chat Service -> Transfrom server data into bytes ...

Er...Can we connect Group Mgr with DAO? **YES**. One point is you can change all diagrams in any time. And another point is, if you don't draw line between Group Mgr and DAO, you can implement the system as well. For example,
1. we can store group info into files
1. we use Chat Service as proxy to store group info. Chat Service will call DAO to store group info, if chat exist. If the group members are all silence, the group will be destroyed after a while.

Is there a formal way to draw those components and lines? **NO.** This is why the software is attractive. **Every software is artwork.** Do you remember your first program? Always print: "Hello World!" It's boring! Can we make the world different?

End? No. I will write how to decompose conponent into classes later.

