---
title: CISCN2021总决赛babypython 
tags: [CTF,Web]
categories: [WebSec]
image:
    path: ../assets/img/image-85.png
---


# 题解思路

伪造session，获取flag


1. ![alt text](../assets/img/image-86.png)

`uuid.getnode()`函数可以获取网卡mac地址并转换成十进制数返回

2. 获取网卡mac地址
    ![alt text](../assets/img/image-87.png)

3. 伪造session
   flask manage session 为flask的session管理器，可以伪造session
