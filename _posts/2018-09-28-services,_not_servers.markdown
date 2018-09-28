---
layout: post
title:  "AWS Services, Not Servers"
date:   2018-09-28 00:00:00 +0000
categories: aws
---

开发，管理和运维应用，尤其时一些对弹性有要求的应用，需要管理很多基础技术组件，在传统IT架构中，每个公司都必须自行构建和运维所有的这些基础组件。现在AWS提供许多计算、存储、数据库、数据分析、应用和部署服务，这些服务可以帮助组织提高效率并且节约费用。不充分利用这些基础设施的话（比如只用EC2），就不能充分利用与计算的优势，从而错失增加开发者产品能力和运维效率的机会。


### Managed Services
aws上提供了许多服务，开发者可以利用他们来提升系统能力，这些服务包括数据库，机器学习，数据分析，队列，搜索，邮件，通知等等。比如SQS，它便宜好用稳定，企业还不用再自行维护队列服务。再比如S3，可以随时随地存储任意数据，而不用担心其容量、硬盘配置、可重用性等等，并且S3还可以作为你网站或者APP的静态资源存储器，提供高可用性的同时，还会自动伸缩来应对访问高峰。同样的例子还有Amazon CloudFront，它可以用来实现内容分发。ELB来实现负载均衡，DynamoDB实现NoSQL，Amazon CloudSearch实现搜索，Amazon Elastic Transcoder实现音视频解码等等


###Serverless Architectures无服务架构
降低运维复杂度的另一个方式就是无服务架构。在这种架构模式中，可以不管理任何服务器等基础设施来给网站、app、数据分析设置IoT
等需求构建事件驱动和异步驱动的服务。这种架构可以缩减费用，因为你不需要为任何基础设施付费，也不需要为了高可用性而为冗余的基础设施付费。


Another approach that can reduce the operational complexity of running applications is that of the serverless architectures. It is possible to build both event-driven and synchronous services for mobile, web, analytics, and the Internet of Things (IoT) without managing any server infrastructure. These architectures can reduce costs because you are not paying for underutilized servers, nor are you provisioning redundant infrastructure to implement high availability.

### 无服务架构的实现方式

- AWS Lambda

只需要将代码上传到AWS Lambda就行。AWS Lambda是通过代码的执行时间和次数来计费的。

- Amazon API Gateway
它运行在AWS Lambda上，使用API Gateway可以构建几乎可以无限伸缩的异步API，如果结合S3里的静态资源，这种模式几乎可以完整地运行一个网站应用。
