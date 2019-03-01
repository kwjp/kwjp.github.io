---
layout: post
title:  "Value equals to match_parent or fill_parent in dimens"
date:   2019-02-28 00:00:00 +0000
categories: android
---

Use px instead of dp as suggested by Jarett Millard in the comments


```xml
<!--wrap_content-->
<dimen name="common_wrap_content">-2px</dimen>
<!--match_parent-->
<dimen name="common_match_parent">-1px</dimen>
```


Use -1px and -2px instead of dp. The px values will not scale based on the device's screen density. – Jarett Millard Feb 24 '15 at 17:33




