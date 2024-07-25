---
title:  CTFShow Web9
tags: [CTF,WebSec]
categories: [WebSec,CTF]
---

没事打打CTF 也是一种乐趣，这里记录一下一些Web方向的题目，以及解题思路。

=======

```php



<?php
        $flag="";
		$password=$_POST['password'];
		if(strlen($password)>10){
			die("password error");
		}
		$sql="select * from user where username ='admin' and password ='".md5($password,true)."'";
		$result=mysqli_query($con,$sql);
			if(mysqli_num_rows($result)>0){
					while($row=mysqli_fetch_assoc($result)){
						 echo "登陆成功<br>";
						 echo $flag;
					 }
			}
    ?>

```

对于函数md5(string,raw) 第二个参数有以下可选项： TRUE - 原始 16 字符二进制格式 FALSE - 默认。32 字符十六进制数 所以只要md5加密后的16进制转化为二进制时有 'or’xxxx，即可构成闭合语句： username ='admin' and password =‘ ’or 'xxxxx' 成功登陆 这里给出两个符合的字符串 ffifdyop 129581926211651571912466741651878684928 但题目有长度限制，所以输入ffifdyop即可获取flag