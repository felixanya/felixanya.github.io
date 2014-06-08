---
layout: post
title: "golang game of socket"
description: ""
category: 
tags: []
intro: "golang game of socket"
---

以下图片是一个简单的socket的服务

![golang socket](http://felixanya.github.com/img/socket.png)

## 简要说明

这SOCKET服务器，只是一个中间控件，只作为一个client和server的转接
其实就是一个proxy，当然这个proxy是可以增加或减少的。

 * 负责对Client建立socket持久连接. 
 * 保持Client 到Server的有效数据关系. 
 * Client 和Server之间的数据交互转发. 
 * 当Client网络出现异常时 ，保证后续任务不中断的情况下， 让Client可以重新建立连接.
 * 当Client 重复登录时， 将旧的socket连接踢出游戏。 并且决策Server中新旧数据交替方案. 
 * 保证一台服务器， 可以代理2w socket持久连接. 
 * 多个Golang来回切换,保证不会出现问题. 
 
具体实现过程:


 * golang socket的基础编程（主要用到net包）. 
 * 消息队列使用（可以用开源的，例如zmq,nsq等待）.
 * 中间数据缓存memcache,redis,groupCache,mongoDb.


### 项目上的一些体会

一个client连接socket分配两个goroutine,一个读和一个写
当然每个client连接上，都记录它们对应的id和链路
信息要广播的时候，就直接推送给channel
要注意的是channel的坑（buf开多大按自己项目的实际应用去开）
如果生产者大于消费者，会阻塞
所以些消息可以不用

发送信息
       select{
		case memoryMsgChan <- res:
		default:
			fmt.Println("channel full")
		}
       }

当然你也可以用case <-time.After(1*1e9):
这是特别注意的地方
最后是一个心跳包的实现
