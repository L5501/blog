---
title: windows激活
description: 用命令将windows激活
data: 2023-05-16 12:10:00
updata: 2023-05-16 12:10:00
tags: 
  - 电脑知识
categories:
  - 杂类
---
用管理员权限打开DOS命令窗口，输入以下的命令即可

```c
slmgr /skms kms.v0v.bid && slmgr /ato
```

本人win10亲测有效，其他的版本暂时没试过