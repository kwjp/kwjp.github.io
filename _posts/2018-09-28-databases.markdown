---
layout: post
title:  "AWS Databases"
date:   2018-09-28 00:00:00 +0000
categories: aws
---

在传统IT架构中，组织常常受限于数据库和存储技术，这种约束可能时来自授权证书高昂的价格，也可能是受限于对形形色色的数据库引擎的运维支持能力。在aws上，这些约束统统不存在了，可以用开源资源的价格获得企业级性能，这样就能让应用的每一个负载都能使用最合适的数据库技术。

## 如何选择最合适的数据库技术
下面这些问题可以帮你决定选择怎样的数据库技术

- 数据库负载主要是读，还是写，还是说均衡的？每秒的请求次数是多少？当用户数增加时，这些数据是如何增长的。
- 数据量及数据存储时长是多少？你预计数据将如何增长，有可以预见的上限吗？每个对象的平均大小如何？这些数据如何实现权限管理？
- 这些数据存储的持久性有什么要求？你的这些数据还有别的备份吗？
- 数据的时延要求如何？你需要同时支持多少连接？
- 数据模型是什么样的，你想怎样查询数据？你的查询需要多表连接吗？你可以牺牲范式来创建更扁平的数据结构以便于扩展吗？
- 你需要什么功能？你需要强类型控制，还是需要灵活性？是否需要复杂的报表和查询功能？你的开发人员更熟悉关系型数据库还是NoSQL

## 关系型数据库
关系型数据库通常把

关系型数据库里数据通常被标准化成表格华的数据结构，它通常由行和列组成。关系型数据库有强大的查询语句和完整性控制，灵活的索引，还能进行快速高效的多表连接查询。AWS的RDS能够在云端提供便于安装，维护，扩展和使用的高弹性关系型数据库。

###弹性
关系型数据库可以通过垂直弹性来伸缩，比如升级到一个更大的RDS实例，或者增加存储设备的速度和容量。另外海可以考虑使用RDS for Aurora，它是一个在相同硬件设备上吞吐量比标准Mysql更高的数据库引擎。对于重读的应用，还可以增加只读从库的数量来扩展水平弹性，从而打破单个数据库带来的性能限制。


####如何高效利用只读从库

只读从库是主库的同步的复制实例。这样的话，从库有时候也许会漏掉一些最新的事务，应用的设计者需要考虑哪些查询可以而接受可能稍微有点陈旧的数据，这些查询就可以走只读从库查询，其它的查询的话就必须得走主库查询了，另外，从库也不能执行任何写入操作。

#### 关系型数据库的负载均衡
关系型数据库的负载均衡需要考虑数据分区和切片

horizon partitioning横向分区（通常称作sharding）,在这个策略中，每个分区本身都是一个数据存储区，但所有分区都有相同的模式。每个分区被称为碎片并持有一个特定的数据集，例如在一个电子商务应用一组特定客户的订单。


vertical partitioning纵向分区。该策略中，每个分区拥有数据仓库的某些部分数据域（举例来说就是一个数据的某些表分发到不同的数据库上面）。这些数据域可以根据其使用的场景来进行分区。比如说，将经常访问的一些数据库置于一个数据仓库，而不常访问的数据仓库置于另一个数据仓库。

在这种模式下数据会被分散到许多数据库实例上，尽管Amazon RDS降低了数据操作的难度，但是横向分区sharding还是会增加应用的复杂度，应用穿层数据需要修改为能够感知到数据时如何分割的，这样才能直接地到正确的数据库实例上去查询。另外任何模式的修改都需要跨多个数据库才能执行，所以有必要使得这个过程自动化。

### 高可用性High Availability
对于所有的产品化关系型数据库，都强烈推荐使用Amazon RDS Multi-AZ deployment特性，它会给你创建一个跨AZ的同步复制的独立数据库实例。为了防止主库写入失败，Amazon RDS会在失败时做故障转义，将从操作转移到备用库，这些操作完全不需要管理人员的手工介入。故障转移时，主节点会出现短时间的不可用，这时候可以通过容错机制来提高应用弹性，比如利用只读从库来提供只读模式的应用。

### 反面模式Anti-Patterns
如果你的应用基本不需要复杂的连接查询和事务控制（尤其是当你需要把写入负载分发到多个数据库实例时），你就应该考虑使用NoSQL，而不是关系型数据库。

如果你有大的二进制文件需要存储，比如图片，音乐，视频等，这时候你可以使用S3来存储这些文件，然后在数据库里存储他们的元数据就行。



## NoSQL Databases
NoSQL可以分担

NoSQL is a term used to describe databases that trade some of the query and transaction capabilities of relational databases for a more flexible data model that seamlessly scales horizontally. NoSQL databases utilize a variety of data models, including graphs, key-value pairs, and JSON documents. NoSQL databases are widely recognized for ease of development, scalable performance, high availability, and resilience. Amazon DynamoDB is a fast and flexible NoSQL database23 service for applications that need consistent, single-digit millisecond latency at any scale. It is a fully managed cloud database and supports both document and key-value store models.