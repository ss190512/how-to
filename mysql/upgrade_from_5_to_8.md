## Download MySQL Yum Repository.
```
https://dev.mysql.com/downloads/repo/yum/ 
```
## Install MySQL repository.
```
# ls
mysql80-community-release-el7-3.noarch.rpm
# yum install ./mysql80-community-release-el7-3.noarch.rpm 
# yum repolist | grep -i mysql
mysql-connectors-community/x86_64 MySQL Connectors Community                 108
mysql-tools-community/x86_64      MySQL Tools Community                       89
mysql80-community/x86_64          MySQL 8.0 Community Server                  99
```
## Check repository config file. Also, enable ver. 5.7 and disable ver. 8.0.
```
# vi /etc/yum.repos.d/mysql-community.repo
...
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```
## Install mysql server ver. 5.7.
```
# yum info mysql-community-server
# yum install mysql-community-server
# systemctl enable mysqld
# systemctl start mysqld
# systemctl status mysqld
# journalctl -a -u mysqld
-- Logs begin at Thu 2019-05-02 19:13:19 EDT, end at Thu 2019-05-02 19:50:01 EDT. --
May 02 19:47:22 ol73ms systemd[1]: Starting MySQL Server...
May 02 19:47:34 ol73ms systemd[1]: Started MySQL Server.
```
## Change root password.
```
# grep "temporary password" /var/log/mysqld.log 
2019-05-02T23:47:32.152819Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 7dqoo*-dF:io
[root@ol73ms download]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.26
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> alter user 'root'@'localhost' identified by 'superPassword#123';
```
## Pre-checks.
```
mysql> show variables like 'version';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| version       | 5.7.26 |
+---------------+--------+
# mysqlcheck -uroot -p --all-databases --check-upgrade
Enter password: 
mysql.columns_priv                                 OK
mysql.db                                           OK
mysql.engine_cost                                  OK
mysql.event                                        OK
mysql.func                                         OK
mysql.general_log                                  OK
mysql.gtid_executed                                OK
mysql.help_category                                OK
mysql.help_keyword                                 OK
mysql.help_relation                                OK
mysql.help_topic                                   OK
mysql.innodb_index_stats                           OK
mysql.innodb_table_stats                           OK
mysql.ndb_binlog_index                             OK
mysql.plugin                                       OK
mysql.proc                                         OK
mysql.procs_priv                                   OK
mysql.proxies_priv                                 OK
mysql.server_cost                                  OK
mysql.servers                                      OK
mysql.slave_master_info                            OK
mysql.slave_relay_log_info                         OK
mysql.slave_worker_info                            OK
mysql.slow_log                                     OK
mysql.tables_priv                                  OK
mysql.time_zone                                    OK
mysql.time_zone_leap_second                        OK
mysql.time_zone_name                               OK
mysql.time_zone_transition                         OK
mysql.time_zone_transition_type                    OK
mysql.user                                         OK
sys.sys_config                                     OK
# 
```
## Make a backup
```
# mysqldump -uroot -p --all-databases > my_backup.sql
Enter password: 
# head my_backup.sql 
-- MySQL dump 10.13  Distrib 5.7.26, for Linux (x86_64)
--
-- Host: localhost    Database: 
-- ------------------------------------------------------
-- Server version	5.7.26
/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;
# 
```
## Shutdown the database
```
# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 6
Server version: 5.7.26 MySQL Community Server (GPL)
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> set global innodb_fast_shutdown = 0;
# systemctl stop mysqld
```
## Modify repo file.
```
# vi /etc/yum.repos.d/mysql-community.repo
...
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```
## Upgrade step.
```
# yum upgrade
...
Install   20 Packages (+125 Dependent packages)
Upgrade  834 Packages
Total size: 1.4 G
Total download size: 451 M
Is this ok [y/d/N]: y
...
Replaced:
  NetworkManager.x86_64 1:1.4.0-12.el7            caribou.x86_64 0:0.4.16-1.el7                caribou-gtk2-module.x86_64 0:0.4.16-1.el7      
  caribou-gtk3-module.x86_64 0:0.4.16-1.el7       gnome-tweak-tool.noarch 0:3.14.3-2.el7       grub2.x86_64 1:2.02-0.44.0.1.el7               
  grub2-tools.x86_64 1:2.02-0.44.0.1.el7          pyatspi.noarch 0:2.14.0-2.el7                pygobject3.x86_64 0:3.14.0-3.el7               
  pygobject3-base.x86_64 0:3.14.0-3.el7           python-caribou.noarch 0:0.4.16-1.el7         rdma.noarch 0:7.3_4.7_rc2-5.el7                
  usbmuxd.x86_64 0:1.0.8-11.el7                  
Complete!
# systemctl start mysqld
```
## Check new version.
```
# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.16 MySQL Community Server - GPL
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show variables like 'version';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| version       | 8.0.16 |
+---------------+--------+
mysql> select version();
+-----------+
| version() |
+-----------+
| 8.0.16    |
+-----------+
```
