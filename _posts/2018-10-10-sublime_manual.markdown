---
layout: post
title:  "sublime的一些操作记录"
date:   2018-10-10 00:00:00 +0000
categories: sublime
---


### 设置不换行
Preferences-->Settings-User  
然后添加："word_wrap" : false

```json
{
    "font_size": 13,
    "word_wrap" : false,
    "ignored_packages":
    [
        "Markdown",
        "Vintage"
    ]
}
```

### 左边栏快捷键
默认快捷键 <kbd>Ctrl</kbd>+<kbd>k</kbd>+<kbd>b</kbd>

修改为 <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>k</kbd>

Preferences-->Key Bindings-User

```json
[
    { "keys": ["alt+pageup"], "command": "sftp_sync_up" },
    { "keys": ["ctrl+shift+k"], "command": "toggle_side_bar" }
]

```

###快速滚动

 - 到底部(通常查看log时用)

<kbd>Ctrl</kbd>+<kbd>End</kbd>

 - 到顶部

<kbd>Ctrl</kbd>+<kbd>Home</kbd>

