---
layout: post
title: 'cpu优先级调度算法和时间片算法模拟程序'
date: 2006-07-26 09:14
comments: true
tags: ['schedule','algorithm','调度','算法']
---
##最初的算法
```cpp
//System Shedule Algorithm  
//include the Priority First Algorithm and the Time Slice Algorithm  
//Version 2.0  
//author:chillyCreator

#include<string>  
#include<iostream>  
#include<iomanip>  
using namespace std;

class process  
{  
private:  
string name;//the name of process  
int pri;//the priority,the bigger is the higher. the number is from 0 to 99  
int round;// the time slice.  
int cpuTime;//the process used by cpu  
int needTime;//the time need to finish the process  
int state;// 1 is ready, 0 is Run, -1 is Finish  
process* next;//to the next  
public :  
//constructor  
process(string Name,int Pri,int NeedTime,int roundTime = 10,int State = 1,int
CpuTime=0,process * Next = NULL)  
{  
name  = Name;  
pri = Pri;  
round = roundTime;  
cpuTime = CpuTime;  
needTime = NeedTime;  
state = State;  
next = Next;  
}  
//gettors and settors  
string getName()  
{  
return name;  
}  
int getPri()  
{  
return pri;  
}  
int getNeedTime()  
{  
return needTime;  
}  
int getCpuTime()  
{  
return cpuTime;  
}  
int getState()  
{  
return state;  
}  
process* getNext()  
{  
return next;  
}  
void setState(int State)  
{  
state = State;  
}  
void setCpuTime(int Time)  
{  
cpuTime += Time;  
}  
void setNeedTime(int Time)  
{  
needTime -= Time;  
}  
void setPri(int Pri)  
{  
pri -= Pri;  
}  
void setNextProcess(process* Next)  
{  
next = Next;  
}  
void print()  
{  
//for print  
cout <<setw(14)<<name << setw(2)<<state<<setw(7)<<needTime  
<<setw(10)<<pri<<setw(5)<<cpuTime<<endl;  
}  
};

//this is nonpreempt system  
class System  
{  
private:  
process* head;  
process* tail;  
int length;  
//auto test  
void test1(int n)  
{  
head = new process("Head",-1,0,0);  
length = 0;  
process* temp = head;  
for(int i=0;i<n;i++)  
{  
temp->setNextProcess(new process("NameIsForTest "+i,20-i,30+i));  
length++;  
temp = temp->getNext();  
}  
tail = temp;  
}  
//user test  
void test2(int n)  
{  
head = new process("Head",-1,0,0);  
length = 0;  
process* temp = head;  
string name;  
int priority,needtime;  
for(int i=0;i<n;i++)  
{  
cin>>name>>priority>>needtime;  
temp->setNextProcess(new process(name,priority,needtime));  
length++;  
temp = temp->getNext();  
}  
tail = temp;  
}  
public:  
//testN is for the various of tests. 1 is using the default test. 2 is using
the User test.  
System(int testN,int num)  
{  
//num is the number of processes  
if(testN == 1)  
test1(num);  
else  
test2(num);  
}  
//delete a process  
void deleteProcess(process* a)  
{  
process* temp = tail;  
toQueueEnd(a);  
delete a;  
tail = temp;  
tail->setNextProcess(NULL);  
length--;  
}  
void toQueueEnd(process* a)  
{  
if(a->getNext()==NULL)  
return;  
process* temp = findP(a);  
temp->setNextProcess(a->getNext());  
tail->setNextProcess(a);  
tail = a;  
tail->setNextProcess(NULL);  
}  
//put the process to the end of the queue  
void toQueueHead(process* a)  
{  
if(head->getNext()==a)  
return;  
process* temp = findP(a);  
temp->setNextProcess(a->getNext());

if(a==tail)  
tail = temp;  
a->setNextProcess(head->getNext());  
head->setNextProcess(a);  
}  
//find parent  
process* findP(process* a)  
{  
process* temp = head;  
while(temp->getNext() != a)  
temp = temp->getNext();  
return temp;  
}  
//when the process is sheduled into the CPU  
void use(process* pro,int RunTime,int dePriority)  
{  
if(pro->getState()==1)  
{  
if(RunTime < pro->getNeedTime())  
{  
toQueueHead(pro);  
pro->setCpuTime(RunTime);  
pro->setNeedTime(RunTime);  
pro->setPri(dePriority);  
toQueueEnd(pro);  
}  
else  
{  
toQueueHead(pro);  
pro->setCpuTime(pro->getNeedTime());  
pro->setNeedTime(pro->getNeedTime());  
pro->setState(-1);  
cout << endl<<pro->getName()<<" finally use the cpu time is
"<<pro->getCpuTime()<<endl;  
deleteProcess(pro);  
}  
}  
}  
void printAll()  
{  
//All of the process are printed  
cout << setw(14)<<"name " << setw(2)<<"state  "<<setw(5)<<"needTime "  
<<setw(3)<<"pri "<<setw(5)<<"cpuTime  "<<endl;  
for(process* temp = head->getNext();temp!=NULL;temp= temp->getNext())  
{  
temp->print();  
}  
}  
//find the Highest priority  
process* findHighest()  
{  
if(head->getNext()==NULL)  
return NULL;  
process* temp = head->getNext();  
process* first = NULL;  
int i=-1;  
for(;temp!=NULL;temp=temp->getNext())  
{  
if(i < temp->getPri())  
{  
i = temp->getPri();  
first = temp;  
}  
}  
return first;  
}  
// the priority First Algorithm  
void FP()  
{  
while(length > 0)  
{

process* fir = findHighest();  
if(fir != NULL)  
{  
printAll();  
use(fir,10,2);  
}  
}  
}  
//the Time slice Algorithm  
void TS(int RunTime)  
{  
  
while(length>0)  
{  
process* temp = head->getNext();  
printAll();  
if(temp != NULL)  
use(temp,RunTime,0);  
}  
}  
};  
//test  
int main()  
{  
System a(1,4);  
a.FP();  
cout<<endl<<endl<<endl;  
System b(1,4);  
b.TS(10);  
return 0;  
}
```


今天终于把上面的代码调试成功了！

不过感觉还不够好。如果队列能写成一个单独的类就好了。

而且今天编写队列的删除，进程移至队尾和队首的算法时，思路没有整理好。故白白浪费了太多时间。

对于移至队尾的操作应该记住要把最后一个节点的next指针赋为null.

移至队首的操作也要注意队尾的变化。

对于队列某个元素的删除操作,可以先将元素置尾再删除。或者查找指向它的前一个元素，再通过基本的操作进行删除。

这段程序代码使用的是链表队列。所以考虑的事件较多。如果使用数组的话会省些时间。

此外process 类的gettors and settors太多。简单的可以将私有的变量成员变为共有的。不过安全性不高。这个类中有些变量并没有什么作用，只不过为以后方便模拟。

##继续优化cpu调度算法


如果在system类中加一个这样函数
```cpp
//find the process which is not used.  
//the process will be older with the Time.  
void findOld(int RunT)  
{  
const int increasePri = -2;  
static int time=1;  
process* temp = head->getNext();  
while(temp!=NULL)  
{  
if((temp->getCpuTime() - time*RunT) < 0)  
temp->setPri(increasePri);  
temp = temp->getNext();  
}  
time++;  
cout <<time;  
}

而void FT（）算法中这样调用：

// the priority First Algorithm  
void FP()  
{  
int TimeSlice = 10;  
int oldTimer=0;  
while(length > 0)  
{  
  
process* fir = findHighest();  
if(oldTimer%4==1)  
{  
findOld(TimeSlice);  
//  cout << oldTimer;  
}  
if(fir != NULL)  
{  
printAll();  
use(fir,10,2);  
}  
oldTimer++;  
}  
}

```

这样改动后就可以让进程随时间的增加而优先级提高。当一个进程在队列中很长一段时间后，进程就会出现老化情况。为了防止老化（就是很难再得到利用）的发生，过一段时间后就会检测进程。当发现长时间没有用到的进程时，优先级会升高。

不过`void findOld(int RunT)`并不严谨。可能会出现所以进程都提高优先级的情况。所以还要进行优化。

另外在上一次的代码中有可能会出现进程优先级为负数的情况。可以这样修改类process
```cpp

void setPri(int Pri)  
{  
    if(pri-Pri<0)  
        return;  
    if(pri-Pri>100)  
        return;  
    pri -= Pri;  
}

```

