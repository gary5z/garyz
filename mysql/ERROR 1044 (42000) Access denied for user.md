在centos下安装好了mysql，用root帐号连上mysql，然后创建一个数据库，提示：ERROR 1044 (42000): Access denied for user ''@'localhost' to database 'mysql'。网上找了一个比较流行的方法（见方法一），搞定了。今天又用这个试了试，却搞不定，在网上找了半天，终于发现是因为mysql数据库的user表里，存在用户名为空的账户即匿名账户，导致登录的时候是虽然用的是root，但实际是匿名登录的，通过错误提示里的''@'localhost'可以看出来，于是解决办法见方法二。

## 方法一：
1.关闭mysql

`# service mysqld stop`

2.屏蔽权限
   `# mysqld_safe --skip-grant-table`
   屏幕出现： Starting demo from .....
3.新开起一个终端输入
```bash
   # mysql -u root mysql
   mysql> UPDATE user SET Password=PASSWORD('newpassword') where USER='root';
   mysql> FLUSH PRIVILEGES;//记得要这句话，否则如果关闭先前的终端，又会出现原来的错误
   mysql> \q
```

## 方法二：
1.关闭mysql
```bash
   # service mysqld stop
```
2.屏蔽权限
```bash
# mysqld_safe --skip-grant-table

```
   屏幕出现： Starting demo from .....
3.新开起一个终端输入
```bash
   # mysql -u root mysql
   mysql> delete from user where USER='';
   mysql> FLUSH PRIVILEGES;//记得要这句话，否则如果关闭先前的终端，又会出现原来的错误
   mysql> \q
```

参考网址：http://blog.csdn.net/tys1986blueboy/article/details/7056835

 

转自：http://www.cnblogs.com/Anker/p/3551610.html#3679998