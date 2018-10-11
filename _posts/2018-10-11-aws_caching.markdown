---
layout: post
title:  "AWS Caching 缓存"
date:   2018-10-11 00:00:00 +0000
categories: aws
---

缓存可以提升应用性能并提高成本效率，可以在IT架构的多个层级中实现缓存。


### 应用数据缓存 Application Data Caching

应用程序通过优良设计可以存储和查询内存中的缓存，这种缓存高效快速易管理。缓存的信息可以是I/O密集型的数据库查询结果，或者时计算密集型程序的产出结果，当缓存未命中时，应用可以计算或者从数据库中查询，然后将其保存在缓存中给后续的请求使用。当缓存命中时，应用可以直接使用它，这不仅可以降低时延提升最终用户的体验，并且还能减少后端数据库的负载。应用可以控制缓存的时长，在一些场合，对于非常高频的对象，即便是几秒钟的缓存也能大大地降低数据库的负载。

Amazon ElastiCache是便于部署运维和扩展的云端内存缓存。它支持两种开源的内存缓存引擎：Memcached and Redis。

### 边缘缓存 Edge Caching

静态资源(e.g., images, css files, streaming of pre-recorded video)和动态资源(e.g., html response, live video) 的备份可以缓存在Amazon CloudFront中，它是一个在世界各地有很多节点的内容分发系统CDN。边缘缓存可以让你的资源存放在靠近用户的设备上，从而降低时延，能够给应用的终端用户提供高速稳定的传输速率。





Requests for your content are carried back to Amazon S3 or your origin servers. If the origin is running on AWS then requests will be transferred over optimized network paths for a more reliable and consistent experience. Amazon CloudFront can be used to deliver your entire website, including non-cachable content. The benefit in that case is that Amazon CloudFront reuses existing connections between the Amazon CloudFront edge and the origin server reducing connection setup latency for each origin request. Other connection optimizations are also applied to avoid Internet bottlenecks and fully utilize available bandwidth between the edge location and the viewer. This means that Amazon CloudFront can speed-up the delivery of your dynamic content and provide your viewers with a consistent and reliable, yet personalized experience when navigating your web application. Amazon CloudFront also applies the same performance benefits to upload requests as those applied to the requests for downloading dynamic content.