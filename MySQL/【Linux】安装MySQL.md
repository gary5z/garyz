## 说明

安装目录：/opt/mysql-5.6.15

## 下载安装包

从阿里云下载
```bash
[root@iz2ze148jq0xn06jls2jw3z opt]# wget http://oss.aliyuncs.com/aliyunecs/onekey/mysql/mysql-5.6.15-linux-glibc2.5-x86_64.tar.gz
--2018-09-08 10:49:47--  http://oss.aliyuncs.com/aliyunecs/onekey/mysql/mysql-5.6.15-linux-glibc2.5-x86_64.tar.gz
Resolving oss.aliyuncs.com (oss.aliyuncs.com)... 118.178.29.11
Connecting to oss.aliyuncs.com (oss.aliyuncs.com)|118.178.29.11|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 304382512 (290M) [application/x-www-form-urlencoded]
Saving to: ‘mysql-5.6.15-linux-glibc2.5-x86_64.tar.gz’

100%[=====================================================================================================================================================>] 304,382,512 11.5MB/s   in 23s    

2018-09-08 10:50:11 (12.4 MB/s) - ‘mysql-5.6.15-linux-glibc2.5-x86_64.tar.gz’ saved [304382512/304382512]
```

## 解压mysql安装包

```bash
[root@iz2ze148jq0xn06jls2jw3z opt]# tar zxvf mysql-5.6.15-linux-glibc2.5-x86_64.tar.gz

默认目录名为mysql-5.6.15-linux-glibc2.5-x86_64，修改为mysql-5.6.15
[root@iz2ze148jq0xn06jls2jw3z opt]# mv mysql-5.6.15-linux-glibc2.5-x86_64 mysql-5.6.15
```

## 创建mysql用户组和用户

用于设置mysql安装目录文件所有者和所属组

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# groupadd mysql
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# useradd -r -g mysql mysql
```
*useradd -r参数表示mysql用户是系统用户，不可用于登录系统。

## 修改MySQL目录属主

进入mysql文件夹，也就是mysql所在的目录，并更改所属的组和用户
```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# chown -R mysql:mysql *
```

## 移除默认my.cnf配置

```bash
[root@iz2ze148jq0xn06jls2jw3z opt]# mv /etc/my.cnf /etc/my.cnf.bak
```

## 在 etc 下新建配置文件my.cnf

MySQL安装目录：/opt/mysql-5.6.15
```bash
[root@iz2ze148jq0xn06jls2jw3z etc]# vim my.cnf
[client]
port            = 3306
socket          = /tmp/mysql.sock

[mysqld]
# 指定MySQL的安装目录&数据文件目录
basedir=/opt/mysql-5.6.15
datadir=/opt/mysql-5.6.15/data
port            = 3306
socket          = /tmp/mysql.sock
skip-external-locking
key_buffer_size = 16M
max_allowed_packet = 1M
table_open_cache = 64
sort_buffer_size = 512K
net_buffer_length = 8K
read_buffer_size = 256K
read_rnd_buffer_size = 512K
myisam_sort_buffer_size = 8M

# MySQL在Linux环境下，表名默认区分大小写，这里设置为不区分大小写
lower_case_table_names = 1

log-bin=mysql-bin
binlog_format=mixed
server-id       = 1

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

[mysqldump]
quick
max_allowed_packet = 16M

[mysql]
no-auto-rehash

[myisamchk]
key_buffer_size = 20M
sort_buffer_size = 20M
read_buffer = 2M
write_buffer = 2M

[mysqlhotcopy]
interactive-timeout
END
```
MySQL在Linux下，表名默认区分大小写
lower_case_table_names设置为表名不区分大小写

# 安装autoconf库

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# yum -y install autoconf
Loaded plugins: fastestmirror
base                                                                                                                                                                    | 3.6 kB  00:00:00     
epel                                                                                                                                                                    | 3.2 kB  00:00:00     
extras                                                                                                                                                                  | 3.4 kB  00:00:00     
updates                                                                                                                                                                 | 3.4 kB  00:00:00     
(1/7): base/7/x86_64/group_gz                                                                                                                                           | 166 kB  00:00:00     
(2/7): epel/x86_64/updateinfo                                                                                                                                           | 939 kB  00:00:00     
(3/7): epel/x86_64/group_gz                                                                                                                                             |  88 kB  00:00:00     
(4/7): extras/7/x86_64/primary_db                                                                                                                                       | 187 kB  00:00:00     
(5/7): base/7/x86_64/primary_db                                                                                                                                         | 5.9 MB  00:00:00     
(6/7): epel/x86_64/primary                                                                                                                                              | 3.6 MB  00:00:00     
(7/7): updates/7/x86_64/primary_db                                                                                                                                      | 5.2 MB  00:00:00     
Determining fastest mirrors
epel                                                                                                                                                                               12670/12670
Resolving Dependencies
--> Running transaction check
---> Package autoconf.noarch 0:2.69-11.el7 will be installed
--> Processing Dependency: perl(Data::Dumper) for package: autoconf-2.69-11.el7.noarch
--> Running transaction check
---> Package perl-Data-Dumper.x86_64 0:2.145-3.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================================================================================================
 Package                                             Arch                                      Version                                           Repository                               Size
===============================================================================================================================================================================================
Installing:
 autoconf                                            noarch                                    2.69-11.el7                                       base                                    701 k
Installing for dependencies:
 perl-Data-Dumper                                    x86_64                                    2.145-3.el7                                       base                                     47 k

Transaction Summary
===============================================================================================================================================================================================
Install  1 Package (+1 Dependent package)

Total download size: 748 k
Installed size: 2.3 M
Downloading packages:
(1/2): perl-Data-Dumper-2.145-3.el7.x86_64.rpm                                                                                                                          |  47 kB  00:00:00     
(2/2): autoconf-2.69-11.el7.noarch.rpm                                                                                                                                  | 701 kB  00:00:00     
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                          7.4 MB/s | 748 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : perl-Data-Dumper-2.145-3.el7.x86_64                                                                                                                                         1/2 
  Installing : autoconf-2.69-11.el7.noarch                                                                                                                                                 2/2 
  Verifying  : perl-Data-Dumper-2.145-3.el7.x86_64                                                                                                                                         1/2 
  Verifying  : autoconf-2.69-11.el7.noarch                                                                                                                                                 2/2 

Installed:
  autoconf.noarch 0:2.69-11.el7                                                                                                                                                                

Dependency Installed:
  perl-Data-Dumper.x86_64 0:2.145-3.el7                                                                                                                                                        

Complete!
```

# 安装libaio库

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# yum install libaio
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Resolving Dependencies
--> Running transaction check
---> Package libaio.x86_64 0:0.3.109-13.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================================================================================================
 Package                                     Arch                                        Version                                               Repository                                 Size
===============================================================================================================================================================================================
Installing:
 libaio                                      x86_64                                      0.3.109-13.el7                                        base                                       24 k

Transaction Summary
===============================================================================================================================================================================================
Install  1 Package

Total download size: 24 k
Installed size: 38 k
Is this ok [y/d/N]: y
Downloading packages:
libaio-0.3.109-13.el7.x86_64.rpm                                                                                                                                        |  24 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : libaio-0.3.109-13.el7.x86_64                                                                                                                                                1/1 
  Verifying  : libaio-0.3.109-13.el7.x86_64                                                                                                                                                1/1 

Installed:
  libaio.x86_64 0:0.3.109-13.el7                                                                                                                                                               

Complete!
```

## 安装数据库

执行mysql_install_db脚本，对mysql中的data目录进行初始化并创建一些系统表格。注意mysql服务进程mysqld运行时会访问data目录，所以必须由启动mysqld进程的用户（就是我们之前设置的mysql用户）执行这个脚本，或者用root执行，但是加上参数--user=mysql。
```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# ./scripts/mysql_install_db --user=mysql --basedir=/opt/mysql-5.6.15/ --datadir=/opt/mysql-5.6.15/data/
Installing MySQL system tables...2018-09-08 11:16:12 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-09-08 11:16:12 10479 [Note] InnoDB: The InnoDB memory heap is disabled
2018-09-08 11:16:12 10479 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2018-09-08 11:16:12 10479 [Note] InnoDB: Compressed tables use zlib 1.2.3
2018-09-08 11:16:12 10479 [Note] InnoDB: Using Linux native AIO
2018-09-08 11:16:12 10479 [Note] InnoDB: Using CPU crc32 instructions
2018-09-08 11:16:12 10479 [Note] InnoDB: Initializing buffer pool, size = 128.0M
2018-09-08 11:16:12 10479 [Note] InnoDB: Completed initialization of buffer pool
2018-09-08 11:16:12 10479 [Note] InnoDB: The first specified data file ./ibdata1 did not exist: a new database to be created!
2018-09-08 11:16:12 10479 [Note] InnoDB: Setting file ./ibdata1 size to 12 MB
2018-09-08 11:16:12 10479 [Note] InnoDB: Database physically writes the file full: wait...
2018-09-08 11:16:12 10479 [Note] InnoDB: Setting log file ./ib_logfile101 size to 48 MB
2018-09-08 11:16:12 10479 [Note] InnoDB: Setting log file ./ib_logfile1 size to 48 MB
2018-09-08 11:16:12 10479 [Note] InnoDB: Renaming log file ./ib_logfile101 to ./ib_logfile0
2018-09-08 11:16:12 10479 [Warning] InnoDB: New log files created, LSN=45781
2018-09-08 11:16:12 10479 [Note] InnoDB: Doublewrite buffer not found: creating new
2018-09-08 11:16:12 10479 [Note] InnoDB: Doublewrite buffer created
2018-09-08 11:16:12 10479 [Note] InnoDB: 128 rollback segment(s) are active.
2018-09-08 11:16:12 10479 [Warning] InnoDB: Creating foreign key constraint system tables.
2018-09-08 11:16:12 10479 [Note] InnoDB: Foreign key constraint system tables created
2018-09-08 11:16:12 10479 [Note] InnoDB: Creating tablespace and datafile system tables.
2018-09-08 11:16:12 10479 [Note] InnoDB: Tablespace and datafile system tables created.
2018-09-08 11:16:12 10479 [Note] InnoDB: Waiting for purge to start
2018-09-08 11:16:13 10479 [Note] InnoDB: 5.6.15 started; log sequence number 0
2018-09-08 11:16:13 10479 [Note] Binlog end
2018-09-08 11:16:13 10479 [Note] InnoDB: FTS optimize thread exiting.
2018-09-08 11:16:13 10479 [Note] InnoDB: Starting shutdown...
2018-09-08 11:16:14 10479 [Note] InnoDB: Shutdown completed; log sequence number 1625977
OK

Filling help tables...2018-09-08 11:16:14 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-09-08 11:16:14 10502 [Note] InnoDB: The InnoDB memory heap is disabled
2018-09-08 11:16:14 10502 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2018-09-08 11:16:14 10502 [Note] InnoDB: Compressed tables use zlib 1.2.3
2018-09-08 11:16:14 10502 [Note] InnoDB: Using Linux native AIO
2018-09-08 11:16:14 10502 [Note] InnoDB: Using CPU crc32 instructions
2018-09-08 11:16:14 10502 [Note] InnoDB: Initializing buffer pool, size = 128.0M
2018-09-08 11:16:14 10502 [Note] InnoDB: Completed initialization of buffer pool
2018-09-08 11:16:14 10502 [Note] InnoDB: Highest supported file format is Barracuda.
2018-09-08 11:16:14 10502 [Note] InnoDB: 128 rollback segment(s) are active.
2018-09-08 11:16:14 10502 [Note] InnoDB: Waiting for purge to start
2018-09-08 11:16:14 10502 [Note] InnoDB: 5.6.15 started; log sequence number 1625977
2018-09-08 11:16:15 10502 [Note] Binlog end
2018-09-08 11:16:15 10502 [Note] InnoDB: FTS optimize thread exiting.
2018-09-08 11:16:15 10502 [Note] InnoDB: Starting shutdown...
2018-09-08 11:16:16 10502 [Note] InnoDB: Shutdown completed; log sequence number 1625987
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system

PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER !
To do so, start the server, then issue the following commands:

  /opt/mysql-5.6.15//bin/mysqladmin -u root password 'new-password'
  /opt/mysql-5.6.15//bin/mysqladmin -u root -h iz2ze148jq0xn06jls2jw3z password 'new-password'

Alternatively you can run:

  /opt/mysql-5.6.15//bin/mysql_secure_installation

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the manual for more instructions.

You can start the MySQL daemon with:

  cd . ; /opt/mysql-5.6.15//bin/mysqld_safe &

You can test the MySQL daemon with mysql-test-run.pl

  cd mysql-test ; perl mysql-test-run.pl

Please report any problems with the ./bin/mysqlbug script!

The latest information about MySQL is available on the web at

  http://www.mysql.com

Support MySQL by buying support/licenses at http://shop.mysql.com

WARNING: Found existing config file /opt/mysql-5.6.15//my.cnf on the system.
Because this file might be in use, it was not replaced,
but was used in bootstrap (unless you used --defaults-file)
and when you later start the server.
The new default config file was created as /opt/mysql-5.6.15//my-new.cnf,
please compare it with your file and take the changes you need.

WARNING: Default config file /etc/my.cnf exists on the system
This file will be read by default by the MySQL server
If you do not want to use this, either remove it, or use the
--defaults-file argument to mysqld_safe when starting the server
```

## 修改MySQL文件夹权限

*将mysql/目录下除了data/目录的所有文件，改回root用户所有，mysql用户只需作为mysql/data/目录下所有文件的所有者。
```bash
[root@localhost mysql]chown -R root .
[root@localhost mysql]chown -R mysql data
```

## 授予my.cnf最大权限

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# chown 777 /etc/my.cnf
```

## 设置开机自启动服务控制脚本：

### 修改MySQL路径

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# ./support-files/mysql.server
basedir=/opt/mysql-5.6.15
datadir=/opt/mysql-5.6.15/data
```

### 复制启动脚本到资源目录

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# cp ./support-files/mysql.server /etc/rc.d/init.d/mysqld
```

说明：mysql.server其实是mysql启停服务的守护进程脚本。通过vi命令可以查看相关注释说明 # vim mysql.server
```bash
#!/bin/sh
# Copyright Abandoned 1996 TCX DataKonsult AB & Monty Program KB & Detron HB
# This file is public domain and comes with NO WARRANTY of any kind

# MySQL daemon start/stop script.

# Usually this is put in /etc/init.d (at least on machines SYSV R4 based
# systems) and linked to /etc/rc3.d/S99mysql and /etc/rc0.d/K01mysql.
# When this is done the mysql server will be started when the machine is
# started and shut down when the systems goes down.

# Comments to support chkconfig on RedHat Linux
# chkconfig: 2345 64 36
# description: A very fast and reliable SQL database engine.

# Comments to support LSB init script conventions
### BEGIN INIT INFO
# Provides: mysql
# Required-Start: $local_fs $network $remote_fs
# Should-Start: ypbind nscd ldap ntpd xntpd
# Required-Stop: $local_fs $network $remote_fs
# Default-Start:  2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: start and stop MySQL
# Description: MySQL is a very fast and reliable SQL database engine.
### END INIT INFO

# If you install MySQL on some other places than /usr/local/mysql, then you
# have to do one of the following things for this script to work:
#
# - Run this script from within the MySQL installation directory
# - Create a /etc/my.cnf file with the following information:
#   [mysqld]
#   basedir=<path-to-mysql-installation-directory>
# - Add the above to any other configuration file (for example ~/.my.ini)
#   and copy my_print_defaults to /usr/bin
# - Add the path to the mysql-installation-directory to the basedir variable
#   below.
#
# If you want to affect other MySQL variables, you should make your changes
# in the /etc/my.cnf, ~/.my.cnf or other MySQL configuration files.

# If you change base dir, you must also change datadir. These may get
# overwritten by settings in the MySQL configuration files.
```


### 增加mysqld服务控制脚本执行权限

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# chmod +x /etc/rc.d/init.d/mysqld
```

### 将mysqld服务加入到系统服务

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# chkconfig --add mysqld
```

### 检查mysqld服务是否已经生效

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# chkconfig --list mysqld

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

mysqld         	0:off	1:off	2:on	3:on	4:on	5:on	6:off
```
表明mysqld服务已经生效，在2、3、4、5运行级别随系统启动而自动启动，以后可以使用service命令控制mysql的启动和停止

## 启动mysqld
命令为:service mysqld start和service mysqld stop

```bash
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# vim /etc/init.d/mysqld
[root@iz2ze148jq0xn06jls2jw3z mysql-5.6.15]# service mysqld start
Warning: World-writable config file '/etc/my.cnf' is ignored
Starting MySQL.                                            [  OK  ]
```

## 配置环境变量

将mysql的bin目录加入PATH环境变量，编辑 ~/.bash_profile文件

```bash
[root@iz2ze148jq0xn06jls2jw3z local]# vim ~/.bash_profile
在文件最后添加如下信息:
export PATH=$PATH:/opt/mysql-5.6.15/bin
```

## 使环境变量生效

```bash
[root@iz2ze148jq0xn06jls2jw3z local]# source ~/.bash_profile
```

## 登录连接

以root账户登录mysql,默认是没有密码的
```bash
[root@iz2ze148jq0xn06jls2jw3z local]# mysql -uroot -p
Warning: World-writable config file '/etc/my.cnf' is ignored
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.6.15 MySQL Community Server (GPL)

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

## 设置root账户密码

```bash
mysql>use mysql;
mysql>update user set password=password('密码') where user='root' and host='localhost';
mysql>flush privileges;

设置远程主机登录，注意下面的your username 和 your password改成你需要设置的用户和密码
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;
```
