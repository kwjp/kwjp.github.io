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

AWS在共享安全责任模型下运行，其中AWS负责底层云基础架构的安全性，您负责保护您在AWS中部署的工作负载。通过这种方式，你可以通过使用AWS托管服务来减少您的职责范围并专注于您的核心竞争力。比如当你使用aws的服务诸如Amazon RDS, Amazon ElastiCache, Amazon CloudSearch, etc.安全补丁相关的作业就完全变成aws的责任，这不仅可以降低团队的运营开销，还可以减少您的漏洞风险。




### 减少特权访问 Reduce Privileged Access
当你将服务器视为可编程资源时，你也可以从中获得安全方面的好处。你可以随时按需更改你的服务器。你可以取消访客对生产环境的访问权限。当一个实例产生了问题，你可以自动或者手动地替换掉它，然而在你替换实例之前，应该收集并存储足够的日志，以便你能够在开发环境中重新部署并在你的持续开发流程中修复掉这个问题，重点是要明确在云计算架构中服务器是一个临时资源。你可以在Amazon CloudWatch Logs中收集这些信息，如果你需要访问它们但是却没有权限，你可以使用API操作来实时访问，以便尽在需要的时候打开网络进行管理。你可以将这些请求和你的管理系统相结合，这样这些访问请求就变得可追踪并且是在授权后才动态处理的。

另一个安全风险的来源是使用服务账户。在传统的环境中，服务账户通常在授权后持久化存储在一个配置文件中，在aws上，你可以通过IAM角色来给应用授予短期权限，这些授权是自动分发和流转的。对于移动应用，可以利用Amazon Cognito来允许客户端设备通过临时令牌获得对AWS资源的受控访问。对于AWS Management Console用户，您可以类似地通过临时令牌提供临时访问，而不是在您的AWS账户中创建IAM用户。通过这种方式，离职的雇员从你们公司的身份目录中移除后也会失去对aws的访问账户。

### Security as Code
aws不仅能够实施传统IT架构中的安全机制，比如防火墙规则，网络访问控制，子网，操作系统加固等。还能通过定义“Golden Environment.”来用脚本存储这些配置。这意味着你可以通过AWS CloudFormation脚本来存储你的安全策略并能够稳定地部署它。这样就可以从多个项目里总结安全相关的最佳实践并将其构造成一个可持续集成的子模块共享，这样在应用发布流程中就可以进行安全策略测试，并可以自动发现应用之间的区别并修改安全策略。

AWS CloudFormation templates可以用来集中管理安全性，一致性和合规性资源，并可以按需快速实施。你可以通过IAM权限控制产品的查看和修改权限。

### 实时审计 Real-Time Auditing

测试和审计是保持环境安全的关键。
Testing and auditing your environment is key to moving fast while staying safe. Traditional approaches that involve periodic (and often manual or sample-based) checks are not sufficient, especially in agile environments where change is constant. On AWS, it is possible to implement continuous monitoring and automation of controls to minimize exposure to security risks. Services like AWS Config, Amazon Inspector, and AWS Trusted Advisor continually monitor for compliance or vulnerabilities giving you a clear overview of which IT resources are in compliance, and which are not. With AWS Config rules you will also know if some component was out of compliance even for a brief period of time, making both point-in-time and period-in-time audits very effective. You can implement extensive logging for your applications (using Amazon CloudWatch Logs) and for the actual AWS API calls by enabling AWS CloudTrail41. AWS CloudTrail is a web service that records API calls to supported AWS services in your AWS account and delivers a log file to your Amazon S3 bucket. Logs can then be stored in an immutable manner and automatically processed to either notify or even take action on your behalf, protecting your organization from non-compliance. You can use AWS Lambda, Amazon EMR, the Amazon Elasticsearch Service, or third-party tools from the AWS Marketplace to scan logs to detect things like unused permissions, overuse of privileged accounts, usage of keys, anomalous logins, policy violations, and system abuse.







