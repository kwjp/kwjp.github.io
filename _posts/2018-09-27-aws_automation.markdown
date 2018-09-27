---
layout: post
title:  "AWS 自动化"
date:   2018-09-27 00:00:00 +0000
categories: aws
---

在传统IT架构中，你常常不得不手动响应各种各样的事件，当在aws中部署系统时，就有很多机会实现自动化，这不仅可以提升你系统的稳定性，也可以提升组织的效率。

### AWS Elastic Beanstalk
这是在aws上部署应用的最简单的方式。开发者只需要上传他们的代码，AWS Elastic Beanstalk会自动完成所有的细节，比如资源初始化，负载均衡，弹性以及监控。

### Amazon EC2 Auto recovery
可以通过Amazon CloudWatch alarm来监控EC2实例，并且出现故障时自动进行恢复。恢复的实例与源实例完全一致，包括实例ID，私有ip，共有ip以及其他所有实例元数据。然而这个功能仅适用于满足特殊配置的特定实例。而且，在实例恢复期间需要重启，所以内存里的数据都会丢失。

### Auto Scaling
利用Auto Scaling，你可以提高应用的可用性，可以根据你定义的情况来增加或减少EC2的容量。可以使用Auto Scaling来调整EC2实例数量，它可以自动地再访问高峰增加EC2实例数量来保持可用性，然后再不那么忙的时候减少EC2实例数量来节约经费。

### Amazon CloudWatch Alarms
你可以创建一个CloudWatch Alarms，这样当某个特殊的指标超过指定值时就会给你发送一个SNS，这些SNS可以直接执行一个 AWS Lambda function，或者发送一个消息到SQS，或者执行一个POST请求。

### Amazon CloudWatch Events
当特定的系统事件发生时，Amazon CloudWatch Events可以发送一个几乎实时的消息给它的订阅者，使用一些简单的规则，就可以在几分钟内做出响应。这些事件可以分发给一个或多个目标，AWS Lambda functions, Amazon Kinesis streams, Amazon SNS topics等等。


### AWS OpsWorks Lifecycle events
AWS OpsWorks支持完整生命周期的可持续配置。它可以自动更新实例的配置来响应环境的变化。

### AWS Lambda Scheduled events
可以通过常规计划来运行指定的Lambda function