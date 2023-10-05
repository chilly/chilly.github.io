---
layout: post
title: "barchart UDT(二)"
date: 2013-08-20 07:49
comments: true
tags: udt,barchart,udp,tcp 
categories:
- knowledge
- program
---
#前言

 &nbsp;&nbsp;&nbsp;&nbsp;barchart UDT 最近还是做了些实验。发现在内网中，这个表现实在不怎么样。测试了一下内网`字节=32 时间=2ms TTL=60`,外网`字节=32 时间=27ms TTL=53`.这两个环境中UDT表现不好。下面给出一些结论：

#结论
 
1. 内网基本不丢包的情况下， udt在15-30并发时upload表现较好。

2. 在外网偶发丢包率的情况下， UDT在上传中，多并发（并发数不超过15）表现较为优异，会偶尔高于TCP.

3. TCP 在偶发丢包/不丢包 网络状态下，性能稳定，而且不会有IO异常情况。

4. UDT在偶发丢包情况下，会有连接失败/重传超时后连接断开等情况发生。需要修改代码逻辑保证完整的数据传输。

5. UDT在大多数情况下，传输率不及TCP。

<!--more-->

#结论分析：

1. UDT 出现在2004年，TCP产生于1975年。UDT为开源项目，但是测试者较少。TCP经过大量的反复试验和OS的支持，性能稳定。TCP比UDT更为成熟。UDT可能还需要更为细致的测试。

2. UDT在UDP搭建可靠性传输。以datagram作为数据单元，一般小于等于1464(bytes)。，而TCP使用协议栈自动分包(1452 bytes),但是TCP在三次握手时可以得知最小的MSS.如果传输路径中小于MTU 1500，可能会导致UDT变慢。因为发送一个包，如果被分片，只要一个分片未被接受，则整个包被丢弃，为协商情况下的UDT,在丢包率增大的情况下，可能会有更多的重传。(3G,4G网络中MTU 没有相关规定，是由运营商自己设置, 动态调整MSS是有必要的。)

3. UDT 在偶发丢包率的情况下，其协议表现很不稳定，逻辑不能简单等同于TCP。这可能需要在编程过程中做更复杂的处理。 

#实验环境

实验环境：
OS: win7(32位), linux（64位）
网卡：Marvell Yukon 88E8057 PCI-E Gigabit Ethernet Controller
主机配置：Pentium(R) Dual-Core E5500 2.8GHz, 2.8GHz， 内存4G(2.97G可用)
内网 字节=32 时间=2ms TTL=60,外网 字节=32 时间=27ms TTL=53
传送数据量为4M，15M。另外还有echo数据(少量数据情况)。
具体实验数据不再一一列举。不过基本上TCP可以打满带宽，而UDT不行。
特别注意UDT发送数据时会有下面的警告：（在有丢包率的情况下）
警告: com.barchart.udt.ExceptionUDT: UDT Error : 1001 : server does not exist : connect0:connect [id: 0x074f0e4b] UDT 
com.barchart.udt.net.ExceptionSendUDT: UDT Error : -4 : user defined message : UDT send time out [id: 0x074f0e3b] 

这些警告代表着在丢包存在的情况下。UDT和TCP比，性能不稳定，存在协议上的问题。这里的解决方法需要使用多次尝试链接和多次尝试发送。TCP在相应丢包率的情况下，并没有这些例外的情况发生。


