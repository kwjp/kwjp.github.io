---
layout: post
title:  "AWS Removing Single Points of Failure"
date:   2018-10-07 00:00:00 +0000
categories: aws
---

当一个系统能够承受一个或多个组件故障时，它就是高可用的，这些组件包括硬盘，服务器，网络等。实现手段包括在各层级自动恢复或者减少故障。



## 引入冗余 Introducing Redundancy
可以通过引入冗余来消除单点故障。冗余的意思就是对于某个任务有多个可用资源。冗余可以在待机模式实现，也可以在激活模式实现。

在待机冗余模式中，当一个资源出现故障，可以通过故障转移来在数秒中内实现功能回复。故障转移通常需要一些时间来完成，在完成之前功能是不可用的。为了节约经费，冗余资源可以仅在需要的时候自动启动，为了加快鼓掌转移速度和最小化中断，冗余资源也可以一直启动着闲置。待机冗余模式通常用在有状态的组件中，比如数据库。

在激活冗余模式中，负载原来就是分发到多个计算机资源上的，当其中一台故障时，其余的节点可以消耗更多的工作量。比起待机冗余模式，它的资源利用率更高，在故障发生时影响的用户量也最少。



## 故障监测 Detect Failure
故障的检测和修复都应该尽量自动化。你可以使用ELB和Route53来配置健康检查，以便在节点发生故障时将负载转移到健康的终端。另外，也可以通过配置自动扩展来实现替换掉故障节点。当然也可以通过EC2的自动回复功能或者AWS OpsWorks and AWS Elastic Beanstalk功能等。没有一种方式是万能的，确保你收集了足够的log记录，这样你就可以设置警报从而记下来人工干预或者自动响应。


## 如何设计健康检查 Designing good health checks

正确地配置健康检查有助于应用正确而又迅速地相应各种各样的故障，然而错误的健康检查策略可能会实质上地降低你应用的可用率。

在经典的三层架构应用中，可以在ELB服务中配置健康检查。状况检查的目的是可靠地评估后端节点的运行状况。一个简单的TCP health check只能确定终端实例是正常运行的，但是不能保证web应用程序的在正常地运行，因此你得通过web服务能够在一些简单的请求上正常地返回200状态玛来进行健康检查。

在这一层，也不应该配置一些所谓的深层次健康检查，这种检查会太频繁地反悔失败结果。比如如果你还要检查你的应用能否连接数据库，那么当数据库故障时就会导致所有的web服务的健康检查结果失败。分层方法通常是最合适的，深层次检查可以在Route53层进行。


Route53到底是什么





At this layer, it might not be a good idea to configure what is called a deep health check, which is a test that depends on other layers of your application to be successful (this could result in false positives). For example, if your health check also assesses whether the instance can connect to a back end database, you risk marking all of your web servers as unhealthy when that database node becomes shortly unavailable. A layered approach is often the best. A deep health check might be appropriate to implement at the Amazon Route53 level. By running a more holistic check that determines if that environment is able to actually provide the required functionality, you can configure Amazon Route53 to failover to a static version of your website until your database is up and running again.






