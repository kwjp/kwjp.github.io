---
layout: post
title:  "RecyclerView inside ScrollView with wrap_content height"
date:   2019-02-26 00:00:00 +0000
categories: android
---

以下两步必须

##### 1.禁止RecyclerView滑动

```java
LinearLayoutManager linearLayoutManager = new LinearLayoutManager(this) {
    @Override
    public boolean canScrollVertically() {
        return false;
    }
};
detailRv.setLayoutManager(linearLayoutManager);
```



##### 2.RecyclerView外层嵌套RelativeLayout

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/rv_detail"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</RelativeLayout>
```
