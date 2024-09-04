---
title: BurpSuite指南 
tags: [WeSec, SecTools]
categories: [WeSec]
image: 
    path: ../assets/img/image-80.png
    lqip: ../assets/img/image-80.png
---


## 安装

1. 推荐下载最新版本的 BurpSuite，[官方下载地址](https://portswigger.net/burp/releases#professional)
2. 最新的里面自己携带了JDK, 避免JDK版本不匹配的问题
3. 注册机 ![alt text](../assets/img/image-81.png),可以去[Site](https://imli.ink/site/)**ByteSec**获取
4. 修改BurpSuitePro.vmoptions文件内容![alt text](../assets/img/image-82.png) 注意注册机器名称一致
5. 启动![alt text](../assets/img/image-83.png)
6. 为了可以直接使用 Burpsuite 可以修改同名文件下的的
    `BurpSuitePro.vmoptions`

    ```shell
    # Enter one VM parameter per line
    # For example, to adjust the maximum memory usage to 512 MB, uncomment the following line:
    # -Xmx512m
    # To include another file, uncomment the following line:
    # -include-options [path to other .vmoption file]

    -XX:MaxRAMPercentage=50
    -include-options settings.vmoptions
    -include-options user.vmoptions
    --add-opens=java.desktop/javax.swing=ALL-UNNAMED
    --add-opens=java.base/java.lang=ALL-UNNAMED
    --add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED
    --add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED
    --add-opens=java.base/jdk.internal.org.objectweb.asm.Opcodes=ALL-UNNAMED
    -javaagent:BurpLoaderKeygen.jar
    -noverify
    -Xmx2048m
    ```


## 界面

![alt text](../assets/img/image-78.png)

1. 看板
2. 目标
3. 代理
4. 重放
5. 外带
6. 随机检测
7. 编码
8. 对比
9. 日志
10. 扩展

## 看板 Dashboard

1. 扫描和实时任务