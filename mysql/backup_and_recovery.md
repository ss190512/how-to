## Backup.
```
[root@ol76ms ~]# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.16 MySQL Community Server - GPL
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> create database ss_db;
Query OK, 1 row affected (0.00 sec)
mysql> use ss_db;
Database changed
mysql> create table test (id integer, ss_text varchar(50));
Query OK, 0 rows affected (0.02 sec)
mysql> insert into test(id, ss_test) values(1, 'First line');
ERROR 1054 (42S22): Unknown column 'ss_test' in 'field list'
mysql> insert into test values(1, 'First line');
Query OK, 1 row affected (0.00 sec)
mysql> insert into test values(2, 'Second line');
Query OK, 1 row affected (0.00 sec)
mysql> select * from test;
+------+-------------+
| id   | ss_text     |
+------+-------------+
|    1 | First line  |
|    2 | Second line |
+------+-------------+
2 rows in set (0.00 sec)
mysql> exit
Bye

[root@ol76ms ~]# mysqldump -uroot -p'superPassword#12345' --all-databases --single-transaction --result-file my_backup.sql
mysqldump: [Warning] Using a password on the command line interface can be insecure.
[root@ol76ms ~]# ll my_backup.sql
-rw-r--r--. 1 root root 921555 Jun 11 01:47 my_backup.sql
```
## Recovery.
```
[root@ol76ms ~]# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 14
Server version: 8.0.16 MySQL Community Server - GPL
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| ss_db              |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
mysql> use ss_db
mysql> select * from test;
+------+-------------+
| id   | ss_text     |
+------+-------------+
|    1 | First line  |
|    2 | Second line |
+------+-------------+
2 rows in set (0.00 sec)
mysql> insert into test values(3, 'Line #3');
Query OK, 1 row affected (0.00 sec)
mysql> insert into test values(4, 'Line #4');
Query OK, 1 row affected (0.00 sec)
mysql> select * from test;
+------+-------------+
| id   | ss_text     |
+------+-------------+
|    1 | First line  |
|    2 | Second line |
|    3 | Line #3     |
|    4 | Line #4     |
+------+-------------+
4 rows in set (0.00 sec)

mysql> source my_backup.sql
...
Query OK, 0 rows affected (0.00 sec)
Query OK, 0 rows affected (0.00 sec)
Query OK, 0 rows affected (0.00 sec)
Query OK, 0 rows affected (0.00 sec)
mysql> select * from test;
+------+-------------+
| id   | ss_text     |
+------+-------------+
|    1 | First line  |
|    2 | Second line |
+------+-------------+
2 rows in set (0.00 sec)
```
