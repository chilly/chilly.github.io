---
title: Class-Diagram-Architect-week3-whatsapp
date: 2017-09-24 22:18:23
tags: ['Architect','whatsapp','leetcode','week3','architect','data model','class diagram']
categories:
- architect

---
I will write an article to design Class Diagram. [In before article, I draw some usecases and component diagrams.](../../Architect-week3-whatsapp/)

# 4. Transform component diagram into class diagram

## 4.1 Mark noun as entity

I like OOM(Object Oriented Model). Because our world is built by things. And we describe those things as noun, like sky, bird, hand, clothes. In OOD(Object Oriented Design), things are object/class entities. So there is the equation: 

```
        things == noun == entities     (4.1.1)
```

Here, I will describe our system again. **Clients** will send **message** to our "Messager Service". **User** will chat with someone. User can chat with many people in one **chat room**. **Chatting** is real-time. But if all clients of user are offline, system will send offline **messages** to one of user's mobile client. 


Why mobile client? Because web/pc client are hardly woke up. Emmm...Should we add a constraint? **"User only use one client to login/chat."** Without this constraint, system should boradcast chat messages?


But here our key point is marking noun. I already marking them as **"BOLD"**. But **client** is not in "Server" rectangle in component diagram. I will draw client class diagram later.

<div align=center>
![class-init.png](/images/Architect-week3-whatsapp/classes-init.png)
**Figure 4.1.1** init classes
</div>


## 4.2 Add relationship between entities

In Figure 4.1.1 Chatting class is the most important class. But Chatting means a chat message. So I change it to be "ChatMsg". In ChatMsg class, there should be who send the message, who will receive it, and what's the status of the message. Many users will receive the same ChatMsg Object. And one ChatMsg only contain one Message. If one ChatMsg is not exist, can the Message in it be exist? I think there should be strong relationship between Message and ChatMsg. And I call the relationship as "co-exist". I draw solid diamond line between ChatMsg and Message. Other relationship with ChatMsg is not co-exist, but without User, the ChatMsg is meaningless. So I draw hollow diamond line between User and ChatMsg. Status is addition feature of ChatMsg. Without Status, we can not show chat status to our users. I draw arrow line between them. Now we draw Figure 4.2.1

<div align=center>
![class-init.png](/images/Architect-week3-whatsapp/classes-init2.png)
**Figure 4.2.1** init classes 2
</div>

Emmm...Does ChatRoom relate with ChatMsg? Yes. We consider users chat with each other in one chat room. When one chat message generated, the system will send it to the users who are in the ChatRoom. So I put Collection<User> into ChatRoom. So here it is:

<div align=center>
![class-init.png](/images/Architect-week3-whatsapp/classes-init3.png)
**Figure 4.2.2** init classes 3
</div>

## 4.3 Think twice

Here, another problem should be considered. If we join/leave ChatRoom, the users of ChatRoom will be changed. Can we see ChatMsg even if we leave the ChatRoom?  If the status of ChatMsg is sent, and then someone join the chatroom, should  the ChatMsg be delivered to him? Can new comer see old ChatMsgs?
There are many requirements:
1. new comer can see old chat messages. But someone quit the room, he can not see any chat messages be occurred in the room.
2. new comer can not see old chat messages. And someone quit the room, he will see nothing chat messages.
3. new comer can not see old chat messages. But even someone quit the room, he can see chat messages before he left in any time.

And there will be corresponding solutions too! 
1. Do nothing, the class digram supports the requirements 1.
2. If ChatRoom is changed, we log it. So ChatRoom should contain version. The solution also supports requirement 1 and 3. But the performance of the system will be lower than 1 or 3.
3. We store Collection&lt;User&gt; in each ChatMsg Object, and Collection&lt;User&gt; is only the snapshot of ChatRoom. It seems there is no relationship between ChatMsg and ChatRoom. And it will cost more spaces (memory/disk) than 1 or 2.

I will choose solution 2. The solution can adapt many requirements, and it stores the relationship between ChatRoom and ChatMsg. And I fill more field of Class Message and User. I call "ChatRoom history" as "ChatRoomSnapshot". When ChatRoom is changing, the system will generate ChatRoomSnapshot to store old ChatRoom. The id of ChatRoomSnapshot and related ChatRoom must be the same. So the relationship between ChatRoom and ChatRoomSnapshot is weak. I just draw a line between them. In Message, I prefer JSONObject as its content. 

<div align=center>
![class-init.png](/images/Architect-week3-whatsapp/classes-init4.png)
**Figure 4.3.1** init classes 4
</div>

All of above are our system "Server" entities. Is it enough? Those entities seem like static or immutable. And in component diagram, there are many components. Which component is related with the class diagram? The answer is none. Figure 4.3.1 only draw basic entities. Those entities will be used in each components. And now, I will draw details of each component, and "give life" to those entities.


