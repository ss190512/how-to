## Server system and status variables.
```
mysql> show status like 'Bytes_received';
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| Bytes_received | 361   |
+----------------+-------+

mysql> show global status like 'Bytes_received';
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| Bytes_received | 1405  |
+----------------+-------+

mysql> show global variables;
...
------------------------+
550 rows in set (0.02 sec)

mysql> show global variables like 'max_connections';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 151   |
+-----------------+-------+
```
## Recommended size of innodb_buffer_pool_size in Gb.
```
mysql> SELECT CEILING(Total_InnoDB_Bytes*1.6/POWER(1024,3)) RIBPS FROM (SELECT SUM(data_length+index_length) Total_InnoDB_Bytes FROM information_schema.tables WHERE engine='InnoDB') A;
+-------+
| RIBPS |
+-------+
|     1 |
+-------+
```
## Check memory.
```
mysql> show global variables like '%buffer_pool_size%';
+-------------------------+---------+
| Variable_name           | Value   |
+-------------------------+---------+
| innodb_buffer_pool_size | 8388608 |
+-------------------------+---------+

/c/ProgramData/MySQL/MySQL Server 8.0
$ cat my.ini  | grep innodb_buffer_pool_size
innodb_buffer_pool_size=8M

mysql> SELECT * FROM performance_schema.memory_summary_global_by_event_name WHERE EVENT_NAME LIKE 'memory/innodb/buf_buf_pool'\G
*************************** 1. row ***************************
                  EVENT_NAME: memory/innodb/buf_buf_pool
                 COUNT_ALLOC: 1
                  COUNT_FREE: 0
   SUM_NUMBER_OF_BYTES_ALLOC: 8585216
    SUM_NUMBER_OF_BYTES_FREE: 0
              LOW_COUNT_USED: 0
          CURRENT_COUNT_USED: 1
             HIGH_COUNT_USED: 1
    LOW_NUMBER_OF_BYTES_USED: 0
CURRENT_NUMBER_OF_BYTES_USED: 8585216
   HIGH_NUMBER_OF_BYTES_USED: 8585216
1 row in set (0.01 sec)

mysql> SELECT * FROM sys.memory_global_by_current_bytes        WHERE event_name LIKE 'memory/innodb/buf_buf_pool'\G
*************************** 1. row ***************************
       event_name: memory/innodb/buf_buf_pool
    current_count: 1
    current_alloc: 8.19 MiB
current_avg_alloc: 8.19 MiB
       high_count: 1
       high_alloc: 8.19 MiB
   high_avg_alloc: 8.19 MiB
1 row in set, 1 warning (0.04 sec)

mysql> SELECT SUBSTRING_INDEX(event_name,'/',2) AS code_area, sys.format_bytes(SUM(current_alloc)) AS current_alloc FROM sys.x$memory_global_by_current_bytes GROUP BY SUBSTRING_INDEX(event_name,'/',2) ORDER BY SUM(current_alloc) DESC;
+---------------------------+---------------+
| code_area                 | current_alloc |
+---------------------------+---------------+
| memory/performance_schema | 266.58 MiB    |
| memory/innodb             | 44.35 MiB     |
| memory/mysys              | 8.57 MiB      |
| memory/sql                | 4.64 MiB      |
| memory/temptable          | 1.00 MiB      |
| memory/mysqld_openssl     | 172.27 KiB    |
| memory/mysqlx             | 2.87 KiB      |
| memory/myisam             | 688 bytes     |
| memory/blackhole          | 312 bytes     |
| memory/csv                | 312 bytes     |
| memory/vio                | 16 bytes      |
+---------------------------+---------------+
11 rows in set (0.01 sec)
```
## Change innodb_buffer_pool_size size permanently(persist) and temporary(global).
```
mysql> set persist innodb_buffer_pool_size=67108864;
Query OK, 0 rows affected (0.02 sec)

mysql> set global innodb_buffer_pool_size=67108864;
Query OK, 0 rows affected (0.00 sec)
```
