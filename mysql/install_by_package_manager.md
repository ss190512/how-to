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
## Check repository config file.
```
# vi /etc/yum.repos.d/mysql-community.repo
...
[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```
## Install mysql server.
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
# ps -ef | grep mysql | grep -v grep
mysql    23162     1  0 17:48 ?        00:00:07 /usr/sbin/mysqld
```
## Change root password.
```
# grep "temporary password" /var/log/mysqld.log 
2019-05-02T23:47:32.152819Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 7dqoo*-dF:io
[root@ol73ms download]# mysql -u root -p 
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.16
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> alter user 'root'@'localhost' identified by 'superPassword#123';
Query OK, 0 rows affected (0.03 sec)
```
