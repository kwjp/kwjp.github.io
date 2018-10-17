---
layout: post
title:  "AWS Certified Solutions Architect Official Study Guide 笔记"
date:   2018-10-16 00:00:00 +0000
categories: aws
---

### s3

 - Each Amazon S3 bucket is created in a specific region that you choose.data in an Amazon S3 bucket is stored in that region unless you explicitly copy it to another bucket located in a different region.
 - Every object stored in an S3 bucket is identified by a unique identifier called a key. You can think of the key as a filename. A key can be up to 1024 bytes of Unicode UTF-8 characters,including embedded slashes, backslashes, dots, and dashes.Keys must be unique within a single bucket, but different buckets can contain objects with the same key.
 - All Amazon S3 objects by default are private, meaning that only the owner has access.
 - If you are using Amazon S3 in a GET-intensive mode, such as a static website hosting, for best performance you should consider using an Amazon CloudFront distribution as a caching layer in front of your Amazon S3 bucket.

### Amazon Glacier

 - Amazon Glacier is extremely durable, storing data on multiple devices across multiple facilities in a region. Amazon Glacier is designed for 99.999999999% durability of objects over a given year.


### 练习题
第二章第12题
复制为什么不能防止误删或恶意删除



