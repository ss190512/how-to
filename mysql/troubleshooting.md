```
/c/ProgramData/MySQL/MySQL Server 8.0
$ cat my.ini | grep log-error
log-error="LM000132862.err"

sergey.solodilov@LM000132862 MINGW64 /c/ProgramData/MySQL/MySQL Server 8.0/Data
$ tail LM000132862.err
...
2019-06-03T19:00:20.771514Z 0 [System] [MY-010931] [Server] C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqld.exe: ready for connections. Version: '8.0.16'  socket: ''  port: 3306  MySQL Community Server - GPL.
2019-06-03T19:00:21.005514Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060

[root@ol76ms ~]# tail /var/log/mysqld.log
...
2019-05-16T21:48:26.839762Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: '/var/run/mysqld/mysqlx.sock' bind-address: '::' port: 33060

mysql> show global variables like 'log_error';
+---------------+-------------------+
| Variable_name | Value             |
+---------------+-------------------+
| log_error     | .\LM000132862.err |
+---------------+-------------------+
```
## Check mysql status.
```
[root@ol76ms ~]# systemctl status mysqld
? mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2019-05-16 17:48:26 EDT; 2 weeks 4 days ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 23080 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 23162 (mysqld)
   Status: "SERVER_OPERATING"
    Tasks: 38
   CGroup: /system.slice/mysqld.service
           +-23162 /usr/sbin/mysqld
May 16 17:48:13 ol76ms systemd[1]: Starting MySQL Server...
May 16 17:48:26 ol76ms systemd[1]: Started MySQL Server.
```
## Check by netstat.
```
[root@ol76ms ~]# netstat -tunlp | grep mysql
tcp6       0      0 :::33060                :::*                    LISTEN      23162/mysqld
tcp6       0      0 :::3306                 :::*                    LISTEN      23162/mysqld
```
## Reset root password.
```
[root@ol76ms ~]# systemctl stop mysqld
[root@ol76ms ~]# mysqld --skip-grant-tables --user=mysql &
[1] 24566
[root@ol76ms ~]# ps -ef | grep mysql | grep -v grep
mysql    24566 24209  3 18:40 pts/0    00:00:00 mysqld --skip-grant-tables --user=mysql
[root@ol76ms ~]# mysql -uroot
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 8.0.16 MySQL Community Server - GPL
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> alter user 'root'@'localhost' identified by 'superPassword#12345';
Query OK, 0 rows affected (0.00 sec)
```
## Change max_connections variable.
```
mysql> show global variables like 'max_connections';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 151   |
+-----------------+-------+
mysql> set persist max_connections=200;
mysql> show global variables like 'max_connections';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 200   |
+-----------------+-------+
```
