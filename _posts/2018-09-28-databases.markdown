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
关系型数据库可以通过垂直弹性来伸缩，比如升级到一个更大的RDS实例，或者增加存储设备的速度和容量。另外，如果使用RDS for Aurora，对于


Relational databases can scale vertically (e.g., by upgrading to a larger Amazon RDS DB instance or adding more and faster storage). In addition, consider the use of Amazon RDS for Aurora, which is a database engine designed to deliver much higher throughput compared to standard MySQL running on the same hardware. For read-heavy applications, you can also horizontally scale beyond the capacity constraints of a single DB instance by creating one or more read replicas.




