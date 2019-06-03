```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)
mysql> use information_schema;
Database changed
mysql> describe tables;
+-----------------+--------------------------------------------------------------------+------+---------+
| Field           | Type                                                               | Null | Default |
+-----------------+--------------------------------------------------------------------+------+---------+
| TABLE_CATALOG   | varchar(64)                                                        | YES  | NULL    |
| TABLE_SCHEMA    | varchar(64)                                                        | YES  | NULL    |
| TABLE_NAME      | varchar(64)                                                        | YES  | NULL    |
| TABLE_TYPE      | enum('BASE TABLE','VIEW','SYSTEM VIEW')                            | NO   | NULL    |
| ENGINE          | varchar(64)                                                        | YES  | NULL    |
| VERSION         | int(2)                                                             | YES  | NULL    |
| ROW_FORMAT      | enum('Fixed','Dynamic','Compressed','Redundant','Compact','Paged') | YES  | NULL    |
| TABLE_ROWS      | bigint(21) unsigned                                                | YES  | NULL    |
| AVG_ROW_LENGTH  | bigint(21) unsigned                                                | YES  | NULL    |
| DATA_LENGTH     | bigint(21) unsigned                                                | YES  | NULL    |
| MAX_DATA_LENGTH | bigint(21) unsigned                                                | YES  | NULL    |
| INDEX_LENGTH    | bigint(21) unsigned                                                | YES  | NULL    |
| DATA_FREE       | bigint(21) unsigned                                                | YES  | NULL    |
| AUTO_INCREMENT  | bigint(21) unsigned                                                | YES  | NULL    |
| CREATE_TIME     | timestamp                                                          | NO   | NULL    |
| UPDATE_TIME     | datetime                                                           | YES  | NULL    |
| CHECK_TIME      | datetime                                                           | YES  | NULL    |
| TABLE_COLLATION | varchar(64)                                                        | YES  | NULL    |
| CHECKSUM        | bigint(21)                                                         | YES  | NULL    |
| CREATE_OPTIONS  | varchar(256)                                                       | YES  | NULL    |
| TABLE_COMMENT   | text                                                               | YES  | NULL    |
+-----------------+--------------------------------------------------------------------+------+---------+
21 rows in set (0.02 sec)
mysql> select table_name, table_schema, table_rows from tables where table_schema='performance_schema';
+------------------------------------------------------+--------------------+------------+
| TABLE_NAME                                           | TABLE_SCHEMA       | TABLE_ROWS |
+------------------------------------------------------+--------------------+------------+
| accounts                                             | performance_schema |        128 |
| cond_instances                                       | performance_schema |        256 |
...
| users                                                | performance_schema |        128 |
| variables_by_thread                                  | performance_schema |     150272 |
| variables_info                                       | performance_schema |        587 |
+------------------------------------------------------+--------------------+------------+
103 rows in set (0.13 sec)
mysql> describe schemata;
+----------------------------+------------------+------+-----+---------+-------+
| Field                      | Type             | Null | Key | Default | Extra |
+----------------------------+------------------+------+-----+---------+-------+
| CATALOG_NAME               | varchar(64)      | YES  |     | NULL    |       |
| SCHEMA_NAME                | varchar(64)      | YES  |     | NULL    |       |
| DEFAULT_CHARACTER_SET_NAME | varchar(64)      | NO   |     | NULL    |       |
| DEFAULT_COLLATION_NAME     | varchar(64)      | NO   |     | NULL    |       |
| SQL_PATH                   | binary(0)        | YES  |     | NULL    |       |
| DEFAULT_ENCRYPTION         | enum('NO','YES') | NO   |     | NULL    |       |
+----------------------------+------------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
mysql> select * from schemata;
+--------------+--------------------+----------------------------+------------------------+----------+--------------------+
| CATALOG_NAME | SCHEMA_NAME        | DEFAULT_CHARACTER_SET_NAME | DEFAULT_COLLATION_NAME | SQL_PATH | DEFAULT_ENCRYPTION |
+--------------+--------------------+----------------------------+------------------------+----------+--------------------+
| def          | mysql              | utf8mb4                    | utf8mb4_0900_ai_ci     |     NULL | NO                 |
| def          | information_schema | utf8                       | utf8_general_ci        |     NULL | NO                 |
| def          | performance_schema | utf8mb4                    | utf8mb4_0900_ai_ci     |     NULL | NO                 |
| def          | sys                | utf8mb4                    | utf8mb4_0900_ai_ci     |     NULL | NO                 |
+--------------+--------------------+----------------------------+------------------------+----------+--------------------+
4 rows in set (0.00 sec)
mysql> describe files;
+----------------------+--------------+------+-----+---------+-------+
| Field                | Type         | Null | Key | Default | Extra |
+----------------------+--------------+------+-----+---------+-------+
| FILE_ID              | bigint(21)   | YES  |     | NULL    |       |
| FILE_NAME            | text         | YES  |     | NULL    |       |
| FILE_TYPE            | varchar(256) | YES  |     | NULL    |       |
| TABLESPACE_NAME      | varchar(268) | NO   |     | NULL    |       |
| TABLE_CATALOG        | char(0)      | NO   |     |         |       |
| TABLE_SCHEMA         | binary(0)    | YES  |     | NULL    |       |
| TABLE_NAME           | binary(0)    | YES  |     | NULL    |       |
...
| CHECKSUM             | binary(0)    | YES  |     | NULL    |       |
| STATUS               | varchar(256) | YES  |     | NULL    |       |
| EXTRA                | varchar(256) | YES  |     | NULL    |       |
+----------------------+--------------+------+-----+---------+-------+
38 rows in set (0.01 sec)

mysql> select file_id, file_name, tablespace_name, engine, status, initial_size from files;
+------------+----------------------+------------------+--------+--------+--------------+
| FILE_ID    | FILE_NAME            | TABLESPACE_NAME  | ENGINE | STATUS | INITIAL_SIZE |
+------------+----------------------+------------------+--------+--------+--------------+
| 4294967294 | ./mysql.ibd          | mysql            | InnoDB | NORMAL |            0 |
|          0 | ./ibdata1            | innodb_system    | InnoDB | NORMAL |     12582912 |
| 4294967293 | ./ibtmp1             | innodb_temporary | InnoDB | NORMAL |     12582912 |
| 4294967279 | ./undo_001           | innodb_undo_001  | InnoDB | NORMAL |     10485760 |
| 4294967278 | ./undo_002           | innodb_undo_002  | InnoDB | NORMAL |     10485760 |
|          1 | ./sys/sys_config.ibd | sys/sys_config   | InnoDB | NORMAL |            0 |
+------------+----------------------+------------------+--------+--------+--------------+
6 rows in set (0.01 sec)
```
