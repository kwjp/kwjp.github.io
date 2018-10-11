---
layout: post
title:  "AWS Security 安全"
date:   2018-10-11 00:00:00 +0000
categories: aws
---

大多数传统IT设施中常用的安全技术和工具都可以被用到云计算中，同时aws还允许你通过各种各样的方式进一步提升安全性。aws平台允许你自行设计安全控制策略。aws简化了管理员和其他IT人员的系统使用，并且以一种可持续的方式使得IT环境易于审查。



### 利用AWS的功能进行深度防御
aws提供了许多功能来帮助架构进行深度防御。从网络级别开始，你可以构建VPC拓扑，通过使用子网，安全组和路由控制来隔离部分基础架构。比如AWS WAF，他是一个网络应用防火墙，可以帮助保护你的Web应用程序免受SQL注入和应用程序代码中的其他漏洞的影响。对于访问控制，你可以利用IAM定义一组精细的策略，并将其应用到用户，群组和aws资源。最后，aws平台提供大量的选项来保护你的数据，不论它是在传输中或者其他状态。

### Offload Security Responsibility to AWS



AWS operates under a shared security responsibility model, where AWS is responsible for the security of the underlying cloud infrastructure and you are responsible for securing the workloads you deploy in AWS. This way, you can reduce the scope of your responsibility and focus on your core competencies through the use of AWS managed services. For example, when you use services such as Amazon RDS, Amazon ElastiCache, Amazon CloudSearch, etc., security patches become the responsibility of AWS. This not only reduces operational overhead for your team, but it could also reduce your exposure to vulnerabilities.