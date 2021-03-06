---
layout: post
title:  "AWS Removing Single Points of Failure"
date:   2018-10-07 00:00:00 +0000
categories: aws
---

当一个系统能够承受一个或多个组件故障时，它就是高可用的，这些组件包括硬盘，服务器，网络等。实现手段包括在各层级自动恢复或者减少故障。



### 引入冗余 Introducing Redundancy
可以通过引入冗余来消除单点故障。冗余的意思就是对于某个任务有多个可用资源。冗余可以在待机模式实现，也可以在被动冗余模式实现。

在被动冗余模式中，当一个资源出现故障，可以通过故障转移来在数秒中内实现功能回复。故障转移通常需要一些时间来完成，在完成之前功能是不可用的。为了节约经费，冗余资源可以仅在需要的时候自动启动，为了加快鼓掌转移速度和最小化中断，冗余资源也可以一直启动着闲置。待机冗余模式通常用在有状态的组件中，比如数据库。

在主动冗余模式中，负载原来就是分发到多个计算机资源上的，当其中一台故障时，其余的节点可以消耗更多的工作量。比起待机冗余模式，它的资源利用率更高，在故障发生时影响的用户量也最少。



### 故障监测 Detect Failure
故障的检测和修复都应该尽量自动化。你可以使用ELB和Route53来配置健康检查，以便在节点发生故障时将负载转移到健康的终端。另外，也可以通过配置自动扩展来实现替换掉故障节点。当然也可以通过EC2的自动回复功能或者AWS OpsWorks and AWS Elastic Beanstalk功能等。没有一种方式是万能的，确保你收集了足够的log记录，这样你就可以设置警报从而记下来人工干预或者自动响应。


### 如何设计健康检查 Designing good health checks

正确地配置健康检查有助于应用正确而又迅速地相应各种各样的故障，然而错误的健康检查策略可能会实质上地降低你应用的可用率。

在经典的三层架构应用中，可以在ELB服务中配置健康检查。状况检查的目的是可靠地评估后端节点的运行状况。一个简单的TCP health check只能确定终端实例是正常运行的，但是不能保证web应用程序的在正常地运行，因此你得通过web服务能够在一些简单的请求上正常地返回200状态玛来进行健康检查。

在这一层，也不应该配置一些所谓的深层次健康检查，这种检查会太频繁地反悔失败结果。比如如果你还要检查你的应用能否连接数据库，那么当数据库故障时就会导致所有的web服务的健康检查结果失败。分层方法通常是最合适的，深层次检查可以在Route53层进行。




### 数据持续存储 Durable Data Storage
应用和用户会产生并需要维护各种各样的数据，数据的可用性和完整性对一个优秀的框架来说是至关重要的，数据复制是引入数据的冗余从机的一种技术，它不仅可以横向扩展增加读的并发量，还能增加数据的持久性和可用性。数据复制可以通过很多种方式来实现。

 - 同步复制  

同步复制模式将数据成功写入主从库视为事务成功的标志，它可以在主节点写入失败时保证数据完整性，同步复制模式可以增加数据库读的并发能力，而且它从库与主库几乎是实时同步的，所以数据的一致性很强。同步复制模式的一个缺点就是主库被从库耦合，写事务在所有从库完成之前不能成功。同步复制模式是数据完整性与可用性的折中方案，尤其是在跨越不可靠或高延迟的连接的拓扑网络中，也正因为如此不建议在同步复制中维持过多的从库实例。

就算不考虑解决方案的数据持久性问题，数据备份始终是不可或缺的。同步复制会将你的所有操作，即便这些操作时软件的bug或者人为错误。然而，存储再s3中的数据可以有版本控制，你可以使用版本控制来保存，恢复或者转储任何版本，使用版本控制，你就可以从意外的用户操作或者程序故障中恢复过来。

 - 异步复制  

异步复制可以将主机点与其副本解耦，其代价是引入了复制延迟，这意味着主节点的变化不会立刻反映到其副本上。异步复制用来横向扩展读取容量，前提是能够接受复制延迟。当然，如果能够接受故障转移时丢失掉一些最近的事务的话，它也能够用来增加数据持久性。比如可以aws跨区域执行异步复制来实现容灾备份。

 - 分布式复制

基于分布式的复制模式结合了同步和异步复制，可以用在大规模的分布式数据库系统中。这种模式可以定义写操作的最少同步节点数。

 - 最佳实践

找到一种适合你的模型很重要，当各种各样的恢复和故障转移发生时，每种方案的恢复点和恢复事件是不一样的。你需要确定你能够接受多少数据的丢失，以及恢复的速度如何。比如Redis engine for Amazon ElastiCache支持自动故障转义，但是Redis engine是异步复制，当发生故障转移时，最近的一些事务就可能丢失了。但是对于Amazon RDS，基于它的Multi AZ feature，它就支持同步复制，从而保证副本数据与主节点同步更新。

### 自动化多数据中心
商用的业务关键型系统需要针对系统故障做更多的保护，这些保护不仅仅时针对硬盘，服务器或者机架。在传统的架构中，你通常需要准备一个容灾方案，当主节点故障时转移到备份节点运行。由于通常两地距离遥远，高延迟导致不太可能去保持两个数据中心同步复制，这就会导致当故障转移时很可能会发生数据丢失或者为数据恢复付出很大的代价，这变成一个不可测的风险。然而，这仍然时针对低概率但是极高风险的一个不错的选择方案，比如当自然灾摧毁了整个IT基础设施长时间不能恢复时。

更可能出现的情况是数据中心的短时间故障，对于短暂故障，因为预期故障时间不长，就很难做出故障转移的决定，通常就避免掉了。aws上可以采用更简单有效的措施来应对这种类型的故障。每个AWS region包含了几个不同的Availability Zones，每个Availability Zones之间进行了故障隔离，Availability Zone就是一个数据中心，有时候一个Availability Zone也可以是多个数据中心。同一个region中的Availability Zone之间提供廉价，低延迟的网络连接，这使得你可以在数据中心之间进行同步数据复制，当故障发生时就可以自动执行故障转移，而这一切对用户来说是透明的。

当然你还可以设计主动冗余（active redundancy）这样你就可以不用为冗余资源付费了。比如应用服务器集群可以跨Availability Zones分布，然后通过ELB分发。当某个Availability Zone内的EC2实例的健康检查失败时，ELB就停止向这个Availability Zone内的节点分发工作量，结合自动扩张，可以在别的Availability Zone中实现平衡而不需要人为干预。

事实上，许多aws的服务是基于Multi-AZ原则设计的，比如说Amazon RDS通过Multi-AZ部署实现高可用性和自动故障转移服务，使用s3和DynamoDB时你的数据也是被冗余存储在多个设备上的。


### 容错 Fault Isolation and Traditional Horizontal Scaling
尽管主动冗余对于负载均衡和处理实例或Availability Zone故障非常有效，但是对于请求本身就有问题的场景就无效了。比如有些情况下所有的实例都会受到影响，如果一个特殊的请求会触发以恶搞bug从而导致系统故障，然后随着故障转移，这个请求又会被发到其他健康的节点，最终导致所有节点都故障。


分片是一种故障隔离方式，它常用在传统的水平扩展中。与传统技术中的分片相似，你也可以将实例分片，而不是将所有的用户请求分发给所有的节点。比如说如果你有8个实例，那就把他们划成4个分片，每个分片2个实例（这两个实例互为冗余），每个用户的请求都会被分发到特定的分片。这样你就可以减少受影响的用户比例。然而这仍然会影响一部分用户，那么关键就在于要进行客户端容错。如果每个客户端都可以尝试与分片资源内的所有服务端建立连接，那么容错能力就能得到巨大的提升，这种技术就叫做随机分片shuffle sharding

