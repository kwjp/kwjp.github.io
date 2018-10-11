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


对内容的请求被发送到S3服务器或者你的源服务器，如果源运行在AWS上，那么这个请求将会被转发到最合适的网络路径，以便用户获得稳定可靠的体验。Amazon CloudFront可以用来分发你的整个网站，包括不能被缓存的内容。这种情况的好处是Amazon CloudFront重用Amazon CloudFront边缘缓存和源服务器之间的现有连接，减少了每个源请求的时延。另外Amazon CloudFront也用来避免互联网瓶颈并最大化利用边缘位置与最终用户之间的带宽。这意味着，在浏览Web应用程序时，Amazon CloudFront可以加速您的动态内容的交付并且，并为查看者提供一致，可靠，个性化的体验。当然Amazon CloudFront不仅可以用于下载动态内容，它也能优化上传的请求。






