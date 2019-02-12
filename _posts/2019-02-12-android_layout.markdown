---
layout: post
title:  "View2紧跟view1，并且不被view1挤出屏幕"
date:   2019-02-12 00:00:00 +0000
categories: android
---

### 实现如题布局，可以参照以下代码

```xml
    <LinearLayout
        android:id="@+id/item_contracting_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="@dimen/margin_horizontal_16dp"
        android:layout_marginTop="@dimen/margin_vertical_16dp"
        android:layout_marginEnd="@dimen/padding_horizontal_16dp"
        android:gravity="center_vertical"
        android:orientation="horizontal">
        <!-- タイトル -->
        <TextView
            android:id="@+id/customer_contracting_item_title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:ellipsize="end"
            android:maxLines="1"
            android:textColor="@color/common_black"
            android:textSize="@dimen/text_size_xx_small"/>
        <!--　ヘルプ　アイコン-->
        <ImageView
            android:id="@+id/customer_help"
            android:layout_width="@dimen/card_title_icon_size"
            android:layout_height="@dimen/card_title_icon_size"
            android:layout_marginStart="@dimen/margin_vertical_1dp"
            android:padding="@dimen/margin_vertical_3dp"
            android:scaleType="fitXY"
            android:src="@drawable/icon_customer_question" />
    </LinearLayout>

```

### 说明

主要是要将view1和view2都放在一个wrap_content的布局中，然后view1设置为warp_content , weight为1
