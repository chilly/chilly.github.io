---
layout: post
title: "python socket send and recv atomticly "
date: 2011-5-4
wordpress_id: 394
comments: true
tags: ["client", "code", "python", "python", "recv", "send", "server", "socket"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="Mood" content="soso" />
<meta name="Weather" content="cloudy" />
<meta name="Doing" content="paper, write c/c++ code and play games" />
<meta name="_su_description" content="this is describe how to create server and client in python. And how to fix bug in simplest server and client." />
<meta name="_su_keywords" content="python,socket,server,client,send,recv" />
<meta name="_su_title" content="python,socket,server,client,send,recv" />
<meta name="views" content="1808" />
first we write simplest client and server.
client:

## in client.py
import socket
class Client:
    def __init__(self, host , port, bufsize = 1024, timeout = 10):
        self.client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.host = host
        self.port  = port
        self.bufsize = bufsize
        self.timeout = timeout
        
    def connect(self):
        self.client.connect((self.host,self.port))
    def close(self):
        self.client.close()
    def send(self, string):
        self.client.send(bytes(string,"utf8"))
    def recv(self):
        return str(self.client.recv(self.bufsize),encoding="utf8")

server:

## in server.py
import socket
class Server:
    def __init__(self,port,listen = 5,timeout = 10, buf = 4096, queueSize = 10):
        self.host = 'localhost'
        self.port = port
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.listen = listen
        self.timeout = timeout
    def send(self, conn, string):
        conn.send(bytes(string,encoding="utf8"))       
    def recv(self, conn):
        return str(conn.recv(self.bufsize),encoding="utf8")
    def run(self):
        print("server start, port:",self.port,"listen num:",self.listen)
        self.sock.bind((self.host, self.port))
        self.sock.listen(self.listen)
        while True:
            connection, address = self.sock.accept()
            message = self.recv(connection)
            print('message',message)
            self.send(connection,'OK')
            connection.close()

next I will write some code to run client and server. Here is :

## in client_start.py
from client import Client
if __name__ == "__main__":
    c = Client('localhost',7777)
    c.connect()
    c.send('hello')
    message = c.recv()
    print('message',message)

## in server_start.py
from server import Server
if __name__ == "__main__":
    s = Server(7777, listen = 1000)
    s.run()

Run server_start.py firstly and then run client_start.py. You will find both client and server print messages. Then client is shut down, but server is still running. Yup. Server should not shut down in order to supply services. And shut client down to close socket connection and save resources of server. These code runs perfectly. But we still should make some improvement. Server can server many clients. When it servers one client connection, it should server other connections and not be held by the former connection. Now multi-thread is considered.

## in "run" function of Server in server.py
## change run function and add a new function called "serviceWaiting"
    def serviceWaiting(self, connection, address):
        print("connection:",id(connection),"address:",str(address)," connect...")
        message = self.recv(connection)
        print('message',message)
        self.send(connection,'OK')
        connection.close()

    def run(self):
        print("server start, port:",self.port,"listen num:",self.listen)
        self.sock.bind((self.host, self.port))
        self.sock.listen(self.listen)
        while True:
            connection, address = self.sock.accept()
            serverThread = Thread(target = self.serviceWaiting, args=(connection, address))
            serverThread.start()

Ok, done! Now server can server multi-clients in the same time. But only sending and receiving string is not enough for our applications.  Sending and receiving objects should be considered.
Here we add two functions in client.py and server.py.

## we should change a function in our former code:
    def recv(self, n= -1):
        if n == -1:
            return str(self.client.recv(self.bufsize),encoding="utf8")
        else:
            return str(self.client.recv(n),encoding="utf8")
## then we should add two function for sending and receive objects
    def recvObj(self):
        OK = 'Y'
        obj = None
        try:
            buffer = []
            totalsize = self.recv()
#            print('client:totalsize',totalsize,'len:',len(totalsize))
            totalsize = int(totalsize)
            self.send(OK)
#            print('client:send length ok')
            while totalsize !=0:
#                print('client: receiving')
                tBuff = self.client.recv(self.bufsize)
                totalsize -= len(tBuff)
#                print('client:tBuff len:',len(tBuff))
                buffer.extend(tBuff) 
            buffer = bytes(buffer)
#            print('client:buff len',len(buffer))
            obj = pickle.loads(buffer)
#            print('client:server receiving done')
#            print('client:obj',obj)
            self.send(OK)
#            print('client:send ok')
        except socket.timeout:
            print ("client:time out")
            raise socket.timeout
        except EOFError:
            print('client:buff',buffer)
            raise EOFError
        return obj
    
    def sendObj(self,  obj):
        OK = 'Y'
        try:
            totalbytes = pickle.dumps(obj)
            totalsize = len(totalbytes)
#            print('client:will send totalsize')
            self.send(str(totalsize))
#            print('client:send totalsize done')
            if self.recv()== OK:
#                print('client:send totalsize success and recv ok')
                self.client.sendall(totalbytes)
#                print('client:send object done')
                string = self.recv(1) 
#                print('client: recv string',string)
                if string == OK:
                    print('client:send object success, and recv ok')
                else:
                    print('client:send object error')
            else:
                print('client:send object error')
        except:
            print('client:send object Exception')

Here I will explain these code. 
	<li>In sendObj(), first transform object to bytes and send the length of object. Then wait to receive "OK" message(string 'Y'). If get OK message, then use sendall() function to send all bytes out. Then wait to receive another OK message. Here we use recv(1) to only get one char in receiving queue. We should not use recv to get more information which may not be used by sendObj() function. </li>
	<li>In recvObj(), first receive the length of object. Then send OK message back. Receive bytes by fixed buffer each time. And receiv many times until we received all bytes of object. Then transform bytes to objects and send OK message back.</li>
Ok. Now we should consider if the we can not connect server at first time. How do we solve this problem? The simplest idea is try many times until we connected server successfully.We should change connection() function.

## in connection function of Client in client.py
    def connect(self, count = -1):
        flag = True
        success = False
        while flag:
            try:
                print('connect...',self.host, self.port)
                
                self.client.connect((self.host,self.port))
                self.client.settimeout(self.timeout)
                success = True
                break
            except:
                count -= 1
                if count == 0:
                    flag = False
                else:
                    time.sleep(3)
        return success

OK. Here we solved many problems, such as multi-clients, sending and receiving objects, and connection failed.

