```
mysql> select user, host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+

mysql> create user 'user1'@'localhost' identified by 'superPassword#12345';
Query OK, 0 rows affected (0.05 sec)

mysql> select user, host from mysql.user where user='user1';
+-------+-----------+
| user  | host      |
+-------+-----------+
| user1 | localhost |
+-------+-----------+

mysql> alter user 'user1'@'localhost' identified by 'newSuperPassword#12345';
Query OK, 0 rows affected (0.03 sec)

mysql> drop user 'user1'@'localhost';
Query OK, 0 rows affected (0.03 sec)

mysql> select user, host from mysql.user where user='user1';
Empty set (0.00 sec)

mysql> create database ss_db;
Query OK, 1 row affected (0.05 sec)

mysql> create user 'ss'@'%' identified by 'superPassword#12345';
mysql> show grants for 'ss'@'%';
+--------------------------------+
| Grants for ss@%                |
+--------------------------------+
| GRANT USAGE ON *.* TO `ss`@`%` |
+--------------------------------+

mysql> grant select, insert, update, delete, create on ss_db.* to 'ss'@'%';
Query OK, 0 rows affected (0.02 sec)

mysql> show grants for 'ss'@'%';
+-----------------------------------------------------------------------+
| Grants for ss@%                                                       |
+-----------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `ss`@`%`                                        |
| GRANT SELECT, INSERT, UPDATE, DELETE, CREATE ON `ss_db`.* TO `ss`@`%` |
+-----------------------------------------------------------------------+

$ mysql -uss -p
Enter password: *******************
...
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| ss_db              |
+--------------------+
2 rows in set (0.00 sec)

mysql> use ss_db;
Database changed

mysql> create table test (id integer, ss_text varchar(50));
Query OK, 0 rows affected (0.17 sec)

mysql> insert into test(id, ss_text) values(1, 'Something 1');
Query OK, 1 row affected (0.03 sec)

mysql> select * from test;
+------+-------------+
| id   | ss_text     |
+------+-------------+
|    1 | Something 1 |
+------+-------------+

mysql> create role dev_ro;
Query OK, 0 rows affected (0.04 sec)
mysql> grant select, insert, update, delete, create on ss_db.* to dev_ro;
Query OK, 0 rows affected (0.04 sec)

mysql> create user 'ss2'@'%' identified by 'superPassword#12345';
mysql> grant dev_ro to 'ss2'@'%';
mysql> SET DEFAULT ROLE ALL TO 'ss2'@'%';
mysql> show grants for 'ss2'@'%';
+---------------------------------+
| Grants for ss2@%                |
+---------------------------------+
| GRANT USAGE ON *.* TO `ss2`@`%` |
| GRANT `dev_ro`@`%` TO `ss2`@`%` |
+---------------------------------+

$ mysql -uss2 -p
...
mysql> SELECT CURRENT_ROLE();
+----------------+
| CURRENT_ROLE() |
+----------------+
| `dev_ro`@`%`   |
+----------------+
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| ss_db              |
+--------------------+
mysql> use ss_db
Database changed
mysql> select * from test;
+------+-------------+
| id   | ss_text     |
+------+-------------+
|    1 | Something 1 |
+------+-------------+
mysql> select CURRENT_USER();
+----------------+
| CURRENT_USER() |
+----------------+
| ss2@%          |
+----------------+
```
