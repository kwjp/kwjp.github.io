---
layout: post
title:  "AWS Optimize for Cost成本优化"
date:   2018-10-11 00:00:00 +0000
categories: aws
---
仅仅是把现有的架构搬到云上，就能借助aws的规模经济来大量消减资本自出以及节约成本。通过迭代和充分利用aws还有更多的机会来创建一个资费优化型的云架构。这篇文章讨论一些云计算费用优化的一些主要原则。



### Right Sizing
aws提供多种资源类型和配置来应对各种各样的用户需求。比如有Amazon EC2, Amazon RDS, Amazon Redshift, and Amazon Elasticsearch Service (Amazon ES) 等各种服务类型可以选择，在有些情况下，你应该选择最便宜的类型来满足你的负载需求，而在另一些情况下，稍微用几台高性能的实例反而能够降低总开支或者获得更好的性能表现。你根据你的工作负载进行评估来选择适合你的资源，这些评估内容包括但不限于CPU, RAM, network, storage size, and I/O。


类似的，你可以通过选择正确的存储方案来节约开支。Amazon S3 提供一系列适合不同使用案例的存储类。其中包括适用于频繁访问数据的通用存储的 S3 标准存储、适用于长时间保存但不频繁访问数据的 S3 标准 – 不频繁访问存储和 S3 单区 – 不频繁访问存储以及适用于长期存档的 Amazon Glacier。Amazon S3 还提供了可配置生命周期策略，以便在数据的生命周期内对其进行管理。一旦设置策略，您的数据便会自动迁移到最合适的存储类别，而您的应用程序无需任何更改。别的服务，比如 Amazon EC2, Amazon RDS, and Amazon ES也是可供选择的选项之一。


### 持续监控和标记Continuous monitoring and tagging 
成本优化是一个持续的迭代过程。你的应用和其功能会随着事件变化，相应的aws也会经常发布新的选择。aws提供工具让你能够节约费用并将你的资源保持在合理的尺寸，为了让这些工具的输出更利于理解，你应该给你的aws资源定义标记。你可以使用AWS Elastic Beanstalk和AWS OpsWorks等AWS管理工具标记构建过程的一部分并使其自动化。你还可以利用AWS Config提供的管理规则来评估特定标记是否应用与你的资源。


### 弹性Elasticity
在aws上省钱的另一个途径就是充分利用平台的弹性。当需求增加时就自动扩展足够多的EC2实例来应对负载，然后在不需要那么大的容量时自动缩减以节约费用。另外，还可以实现自动关闭无负载的实例，最后，考虑一下哪些工作负载可以放到AWS Lambda上，这样你就真的完全不用为任何闲置或冗余资源付费了。

另外，如果可能的话，把EC2的负载用AWS管理的不需要你做任何容量上的决策的服务替换掉（比如ELB, Amazon CloudFront, Amazon SQS, Amazon Kinesis Firehose, AWS Lambda, Amazon SES, Amazon CloudSearch），或者用很容易修改容量的服务替换（比如Amazon DynamoDB, Amazon RDS, Amazon Elasticsearch Service）

### 充分利用各种各样的采购方案Take Advantage of the Variety of Purchasing Options

使用按需实例，您只需为自己使用的 EC2 实例付费。按需实例让您不必面对计划、采购和维护硬件带来的成本和复杂性，并能将一般较高的固定成本变为较低的可变成本。 按需实例价格给你提供最大化的灵活度，并且不需要长期使用的承诺。

##### Amazon EC2 预留实例 Reserved Capacity

与按需实例的定价相比，Amazon EC2 预留实例 (RI) 可提供大幅折扣 (最高可达 75%)，并且在特定可用区中使用时还可提供容量预留。这对可预计最小容量的应用来说是一个完美的选择。你可以通过AWS Trusted Advisor或者Amazon EC2 的使用报告来确定你大多数时候需要使用的容量，这就是你应该保留的容量。基于你购买的保留实例容量，折扣会反映到你的月度账单中。请注意保留实例和按需实例在技术上没有区别，唯一的区别就是你的付费方式。

预留容量方案页适用于aws的其他服务(e.g., Amazon Redshift, Amazon RDS, Amazon DynamoDB, and Amazon CloudFront)

需要注意的是，你的应用在通过充分的生产环境测试之前，不要提交RI的购买订单。在你购买保存容量之后，你可以利用RI利用报告来确保你还在最大化利用你的保存容量。

##### Spot Instances

对于不稳定的负载需求，你可以考虑使用Spot Instances。它允许你竞价使用闲置的ec2计算资源。因为Spot Instances经常会比按需实例便宜很多，所以你可以用它大幅降低应用的成本。

Spot Instances对于能够多次灵活地启动和终止的负载来说非常是一个理想的选择。当你的报价超过了当前的市场价时，你的Spot Instances就会启动起来，当市场价格超过你的报价时它会自动停止，当然你也可以选择主动终止它。如果市场价超过了你的报价，你的实例会自动被终止，这时不满一小时的部分不会被计费。

所以，Spot Instances非常适合能够忍受中断的工作负载。当然，如果你需要更多的可预测的容量时，也可以使用Spot Instances。

竞价策略：你在使用Spot Instance时支付的是市场价格（Spot market price），而不是你的竞价。所以，尽管市场价在预期范围内会出现短期的偶然的高峰，你还是可以报一个比市场价高得多的价格，这样你仍然可以在长期内节约大量费用。

和按需实例混合使用：考虑混合使用Reserved, On-Demand, and Spot Instances，将可预测的最小容量与偶发的访问向结合，使用spot市场价来增加计算机资源，这是增加吞吐容量和提升应用性能的一个好途径。

Spot Blocks for Defined-Duration Workloads为固定时常的工作负载竞价: 你也可以竞价固定使用时长的Spot Instances，如果你的竞价中标，那么你可以使用到该期间结束，或者你也可以手动关闭它。在这种模式下你的实例不会因为市场价格变动而被关闭（当然，你还是得设计好容错机制，因为Spot Instance也会像其他的EC2一样发生故障）


最佳实践 Spot pricing best practices： Spot Instances允许同时竞价多种实例类型，因为价格在Availability Zone中针对每种实例类型独立波动，所以如果你的应用被设计的可以使用多种类型的实例，那么你就可以使用同样的价格获得更多的计算机资源。尽量在多种多样的实例上测试你的应用，然后竞价所有符合你需求的实例类型，这样你就可以进一步降低成本。
