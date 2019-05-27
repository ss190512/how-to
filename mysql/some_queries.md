## Some commands.
```
$ mysql -uroot -p
Enter password: **********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
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
| sys                |
+--------------------+
mysql> use mysql
Database changed

$ mysql -uroot -p -D mysql
Enter password: **********
mysql> SELECT USER();
+----------------+
| USER()         |
+----------------+
| root@localhost |
+----------------+
mysql> SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| mysql      |
+------------+
mysql> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| component                 |
| db                        |
...
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
33 rows in set (0.01 sec)
mysql> describe user;
+--------------------------+-----------------------------------+------+-----+-----------------------+-------+
| Field                    | Type                              | Null | Key | Default               | Extra |
+--------------------------+-----------------------------------+------+-----+-----------------------+-------+
| Host                     | char(60)                          | NO   | PRI |                       |       |
| User                     | char(32)                          | NO   | PRI |                       |       |
| Select_priv              | enum('N','Y')                     | NO   |     | N                     |       |
...
| Password_require_current | enum('N','Y')                     | YES  |     | NULL                  |       |
| User_attributes          | json                              | YES  |     | NULL                  |       |
+--------------------------+-----------------------------------+------+-----+-----------------------+-------+
51 rows in set (0.01 sec)

mysql> show index from user;
+-------+------------+----------+--------------+-------------+-----------+-------------+------------+---------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Index_type | Visible |
+-------+------------+----------+--------------+-------------+-----------+-------------+------------+---------+
| user  |          0 | PRIMARY  |            1 | Host        | A         |           2 | BTREE      | YES     |
| user  |          0 | PRIMARY  |            2 | User        | A         |           5 | BTREE      | YES     |
+-------+------------+----------+--------------+-------------+-----------+-------------+------------+---------+
2 rows in set (0.01 sec)

mysql> SELECT DISTINCT TABLE_NAME, INDEX_NAME FROM INFORMATION_SCHEMA.STATISTICS WHERE TABLE_SCHEMA = 'MYSQL';
+---------------------------+------------+
| TABLE_NAME                | INDEX_NAME |
+---------------------------+------------+
| innodb_table_stats        | PRIMARY    |
| innodb_index_stats        | PRIMARY    |
| db                        | PRIMARY    |
| db                        | User       |
| user                      | PRIMARY    |
...
| engine_cost               | PRIMARY    |
| proxies_priv              | PRIMARY    |
| proxies_priv              | Grantor    |
+---------------------------+------------+
38 rows in set (0.01 sec)

mysql> show status;
...
453 rows in set (0.00 sec)

mysql> show variables;
...
570 rows in set (0.01 sec)

mysql> show variables like '%inno%';
+------------------------------------------+------------------------+
| Variable_name                            | Value                  |
+------------------------------------------+------------------------+
| innodb_adaptive_flushing                 | ON                     |
...
| innodb_write_io_threads                  | 4                      |
+------------------------------------------+------------------------+
136 rows in set (0.01 sec)

mysql> show variables like '%max_connections%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| max_connections        | 151   |
| mysqlx_max_connections | 100   |
+------------------------+-------+

mysql> show engines;
+--------------------+---------+-------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                   | Transactions | XA   | Savepoints |
+--------------------+---------+-------------------------------------------+--------------+------+------------+
| MEMORY             | YES     | Hash based, stored in memory, useful for t| NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables     | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                        | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine            | NULL         | NULL | NULL       |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                        | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                     | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, | YES          | YES  | YES        |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you wri| NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                    | NO           | NO   | NO         |
+--------------------+---------+-------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```

