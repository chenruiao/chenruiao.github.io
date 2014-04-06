---
layout: post
title: "Windows7给C盘扩容"
date: 2013-07-13 19:56
comments: true
categories: [软件使用]
---

在之前的系统中都是使用`PartitionMagic`来进行磁盘容量的分配，但`PartitionMagic`在`Windows7`中的兼容性不是好很好，导致不能使用。其实`Windows7`自带了磁盘管理工具，下面说说怎样使用自带的磁盘管理工具进行C盘的扩容。

1. 将D盘的文件转移到其他的地方；
1. 使用[DiskGenius](http://www.diskgenius.cn/)将D盘转换成主分区，如果你的D盘已经是主分区，这一步可以省略；
1. 删除D盘分区；
1. 打开磁盘管理（计算机右键-》管理-》磁盘管理），在C盘分区上点击右键，这时扩展卷是激活状态，根据向导就可以实现C盘的扩容了。