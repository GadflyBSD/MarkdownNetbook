### 一、在 `MySQL-5.7.11` 以下时使用 `mysqldump` 进行备份
参数|说明|简写
-|-|-
--add-drop-database|在建立库之前先执行删库操作
--add-drop-table|在建表之前先执行删表操作
--add-drop-trigger|在建触发器之前先执行删触发器操作
--add-locks|备份表时，使用LOCK TABLES和UNLOCK TABLES。注意：这个参数不支持并行备份，需要关闭并行备份功能：--default-parallelism=0
--all-databases|备份所有库| -A 
--allow-keywords|Allow creation of column names that are keywords
--apply-slave-statements|Include STOP SLAVE prior to CHANGE MASTER statement and START SLAVE at end of output
--bind-address|指定通过哪个网络接口来连接Mysql服务器（一台服务器可能有多个IP），防止同一个网卡出去影响业务。
--character-sets-dir|Directory where character sets are installed
--comments|Add comments to dump file
--compact|Produce more compact output
--compatible|它告诉 mysqldump，导出的数据将和哪种数据库或哪个旧版本的 MySQL 服务器相兼容。值可以为 ansi、mysql323、mysql40、postgresql、oracle、mssql、db2、maxdb、no_key_options、no_tables_options、no_field_options 等，要使用几个值，用逗号将它们隔开。当然了，它并不保证能完全兼容，而是尽量兼容。
--complete-insert|导出的数据采用包含字段名的完整 INSERT 方式，也就是把所有的值都写在一行。这么做能提高插入效率，但是可能会受到 max_allowed_packet 参数的影响而导致插入失败。因此，需要谨慎使用该参数，至少我不推荐。
--compress|压缩客户端和服务器传输的所有的数据|-C
--create-options|Include all MySQL-specific table options in CREATE TABLE statements
--databases|手动指定要备份的库，支持多个数据库，用空格分隔|-B
--debug|Write debugging log
--debug-check|Print debugging information when program exits
--debug-info|Print debugging information, memory, and CPU statistics when program exits
--default-auth|Authentication plugin to use
--default-character-set|指定备份的字符集。
--defaults-extra-file|Read named option file in addition to usual option files
--defaults-file|Read only named option file
--defaults-group-suffix|Option group suffix value	 	 
--delete-master-logs|On a master replication server, delete the binary logs after performing the dump operation	 	 
--disable-keys|告诉 mysqldump 在 INSERT 语句的开头和结尾增加 /*!40000 ALTER TABLE table DISABLE KEYS */; 和 /*!40000 ALTER TABLE table ENABLE KEYS */; 语句，这能大大提高插入语句的速度，因为它是在插入完所有数据后才重建索引的。该选项只适合 MyISAM 表。	 	 
--dump-date|Include dump date as "Dump completed on" comment if --comments is given	 	 
--dump-slave|Include CHANGE MASTER statement that lists binary log coordinates of slave's master	 	 
--enable-cleartext-plugin|Enable cleartext authentication plugin	5.7.10	 
--events|Dump events from dumped databases	 	 
--extended-insert|默认情况下，mysqldump 开启 --complete-insert 模式，因此不想用它的的话，就使用本选项，设定它的值为 false 即可。 
--fields-enclosed-by|This option is used with the --tab option and has the same meaning as the corresponding clause for LOAD DATA INFILE	 	 
--fields-escaped-by|This option is used with the --tab option and has the same meaning as the corresponding clause for LOAD DATA INFILE	 	 
--fields-optionally-enclosed-by|This option is used with the --tab option and has the same meaning as the corresponding clause for LOAD DATA INFILE	 	 
--fields-terminated-by|This option is used with the --tab option and has the same meaning as the corresponding clause for LOAD DATA INFILE	 	 
--flush-logs|Flush MySQL server log files before starting dump	 	 
--flush-privileges|Emit a FLUSH PRIVILEGES statement after dumping mysql database	 	 
--force|Continue even if an SQL error occurs during a table dump	 	 
--help|Display help message and exit	 	 
--hex-blob|使用十六进制格式导出二进制字符串字段。如果有二进制数据就必须使用本选项。影响到的字段类型有 BINARY、VARBINARY、BLOB。 
--host|Host to connect to (IP address or hostname)	 	 
--ignore-error|Ignore specified errors	5.7.1	 
--ignore-table|Do not dump given table	 	 
--include-master-host-port|Include MASTER_HOST/MASTER_PORT options in CHANGE MASTER statement produced with --dump-slave	 	 
--insert-ignore|Write INSERT IGNORE rather than INSERT statements	 	 
--lines-terminated-by|This option is used with the --tab option and has the same meaning as the corresponding clause for LOAD DATA INFILE	 	 
--lock-all-tables|在开始导出之前，提交请求锁定所有数据库中的所有表，以保证数据的一致性。这是一个全局读锁，并且自动关闭 --single-transaction 和 --lock-tables 选项。|-x 	 
--lock-tables|它和 --lock-all-tables 类似，不过是锁定当前导出的数据表，而不是一下子锁定全部库下的表。本选项只适用于 MyISAM 表，如果是 Innodb 表可以用 --single-transaction 选项。
--log-error|Append warnings and errors to named file	 	 
--login-path|Read login path options from .mylogin.cnf	 	 
--master-data|Write the binary log file name and position to the output	 	 
--max_allowed_packet|Maximum packet length to send to or receive from server	 	 
--net_buffer_length|Buffer size for TCP/IP and socket communication	 	 
--no-autocommit|Enclose the INSERT statements for each dumped table within SET autocommit = 0 and COMMIT statements	 	 
--no-create-db|Do not write CREATE DATABASE statements	 	 
--no-create-info|只导出数据，而不添加 CREATE TABLE 语句。|-t	 	 
--no-data|不导出任何数据，只导出数据库表结构。|-d 	 
--no-defaults|Read no option files	 	 
--no-set-names|Same as --skip-set-charset	 	 
--no-tablespaces|Do not write any CREATE LOGFILE GROUP or CREATE TABLESPACE statements in output	 	 
--opt|这只是一个快捷选项，等同于同时添加 --add-drop-tables --add-locking --create-option --disable-keys --extended-insert --lock-tables --quick --set-charset 选项。本选项能让 mysqldump 很快的导出数据，并且导出的数据能很快导回。该选项默认开启，但可以用 --skip-opt 禁用。注意，如果运行 mysqldump 没有指定 --quick 或 --opt 选项，则会将整个结果集放在内存中。如果导出大数据库的话可能会出现问题。
--order-by-primary|Dump each table's rows sorted by its primary key, or by its first unique index	 	 
--password|备份需要的密码|-p 	 
--pipe|On Windows, connect to server using named pipe	 	 
--plugin-dir|Directory where plugins are installed	 	 
--port|备份数据库的TCP/IP端口		 	 
--print-defaults|Print default options	 	 
--protocol|指定连接服务器的协议	{TCP/SOCKET/PIPE/MEMORY}	 
--quick|该选项在导出大表时很有用，它强制 mysqldump 从服务器查询取得记录直接输出而不是取得所有记录后将它们缓存到内存中。|-q
--quote-names|Quote identifiers within backtick characters	 	 
--replace|Write REPLACE statements rather than INSERT statements	 	 
--result-file|Direct output to a given file	 	 
--routines|导出存储过程以及自定义函数。|-R 	 
--secure-auth|Do not send passwords to server in old (pre-4.1) format	5.7.4	5.7.5
--set-charset|Add SET NAMES default_character_set to output	 	 
--set-gtid-purged|Whether to add SET @@GLOBAL.GTID_PURGED to output	 	 
--shared-memory-base-name|The name of shared memory to use for shared-memory connections	 	 
--single-transaction|该选项在导出数据之前提交一个 BEGIN SQL语句，BEGIN 不会阻塞任何应用程序且能保证导出时数据库的一致性状态。它只适用于事务表，例如 InnoDB 和 BDB。本选项和 --lock-tables 选项是互斥的，因为 LOCK TABLES 会使任何挂起的事务隐含提交。要想导出大表的话，应结合使用 --quick 选项。 	 
--skip-add-drop-table|Do not add a DROP TABLE statement before each CREATE TABLE statement	 	 
--skip-add-locks|Do not add locks	 	 
--skip-comments|Do not add comments to dump file	 	 
--skip-compact|Do not produce more compact output	 	 
--skip-disable-keys|Do not disable keys	 	 
--skip-extended-insert|Turn off extended-insert	 	 
--skip-opt|Turn off options set by --opt	 	 
--skip-quick|Do not retrieve rows for a table from the server a row at a time	 	 
--skip-quote-names|Do not quote identifiers	 	 
--skip-set-charset|Do not write SET NAMES statement	 	 
--skip-triggers|Do not dump triggers	 	 
--skip-tz-utc|Turn off tz-utc	 	 
--socket|For connections to localhost, the Unix socket file to use	 	 
--ssl|Enable encrypted connection	 	 
--ssl-ca|File that contains list of trusted SSL Certificate Authorities	 	 
--ssl-capath|Directory that contains trusted SSL Certificate Authority certificate files	 	 
--ssl-cert|File that contains X509 certificate	 	 
--ssl-cipher|List of permitted ciphers for connection encryption	 	 
--ssl-crl|File that contains certificate revocation lists	 	 
--ssl-crlpath|Directory that contains certificate revocation list files	 	 
--ssl-key|File that contains X509 key	 	 
--ssl-mode|Security state of connection to server	5.7.11	 
--ssl-verify-server-cert|Verify host name against server certificate Common Name identity	 	 
--tab|Produce tab-separated data files	 	 
--tables|指定备份的表，多个用逗号分隔	 	 
--tls-version|Protocols permitted for encrypted connections	5.7.10	 
--triggers|同时导出触发器。该选项默认启用，用 --skip-triggers 禁用它。 	 
--tz-utc|Add SET TIME_ZONE='+00:00' to dump file	 	 
--user|MySQL user name to use when connecting to server	 	 
--verbose|Verbose mode	 	 
--version|Display version information and exit	 	 
--where|Dump only rows selected by given WHERE condition	 	 
--xml|Produce XML output

### 二、在 `MySQL-5.7.11` 及以上版本时使用 `mysqlpump` 进行备份
参数|说明|简写
-|-|-
--add-drop-database|在建立库之前先执行删库操作。	 
--add-drop-table|在建表之前先执行删表操作。 
--add-drop-user|在CREATE USER语句之前增加DROP USER，注意：这个参数需要和--users一起使用，否者不生效。
--add-locks|备份表时，使用LOCK TABLES和UNLOCK TABLES。注意：这个参数不支持并行备份，需要关闭并行备份功能：--default-parallelism=0 
--all-databases|备份所有库|-A
--bind-address|指定通过哪个网络接口来连接Mysql服务器（一台服务器可能有多个IP），防止同一个网卡出去影响业务。 
--character-sets-dir|Directory where character sets are installed	 
--complete-insert|dump出包含所有列的完整insert语句。 
--compress|压缩客户端和服务器传输的所有的数据|-C
--compress-output|默认不压缩输出，目前可以使用的压缩算法有LZ4和ZLIB。 
--databases|手动指定要备份的库，支持多个数据库，用空格分隔|-B	 
--debug	Write|debugging log	 
--debug-check|Print debugging information when program exits	 
--debug-info|Print debugging information, memory, and CPU statistics when program exits	 
--default-auth|Authentication plugin to use	 
--default-character-set|指定备份的字符集 
--default-parallelism|指定并行线程数，默认是2，如果设置成0，表示不使用并行备份。注意：每个线程的备份步骤是：先create table但不建立二级索引（主键会在create table时候建立），再写入数据，最后建立二级索引。
--defaults-extra-file|Read named option file in addition to usual option files	 
--defaults-file|Read only named option file	 
--defaults-group-suffix|Option group suffix value	 
--defer-table-indexes|延迟创建索引，直到所有数据都加载完之后，再创建索引，默认开启。若关闭则会和mysqldump一样：先创建一个表和所有索引，再导入数据，因为在加载还原数据的时候要维护二级索引的开销，导致效率比较低。关闭使用参数：--skip--defer-table-indexes。 
--events|备份数据库的事件，默认开启，关闭使用--skip-events参数。
--exclude-databases|备份排除该参数指定的数据库，多个用逗号分隔 
--exclude-events|备份排除该参数指定的事件，多个用逗号分隔 
--exclude-routines|备份排除该参数指定的自定义函数和存储过程，多个用逗号分隔 
--exclude-tables|备份排除该参数指定的表，多个用逗号分隔 
--exclude-triggers|备份排除该参数指定的触发器，多个用逗号分隔 
--exclude-users|备份排除该参数指定的用户，多个用逗号分隔
--extended-insert|Use multiple-row INSERT syntax	 
--help|Display help message and exit	 
--hex-blob|Dump binary columns using hexadecimal notation	 
--host|Host to connect to (IP address or hostname)	 
--include-databases|指定备份数据库，多个用逗号分隔	 
--include-events|指定备份的事件，多个用逗号分隔 
--include-routines|指定备份的自定义函数和存储过程，多个用逗号分隔	 
--include-tables|指定备份的表，多个用逗号分隔 
--include-triggers|指定备份的触发器，多个用逗号分隔 
--include-users|指定备份的用户，多个用逗号分隔	 
--insert-ignore|备份用insert ignore语句代替insert语句。
--log-error-file|备份出现的warnings和erros信息输出到一个指定的文件	 
--login-path|Read login path options from .mylogin.cnf	 
--max-allowed-packet|备份时用于client/server直接通信的最大buffer包的大小	 
--net-buffer-length|Buffer size for TCP/IP and socket communication	 
--no-create-db|Do not write CREATE DATABASE statements	 
--no-create-info|Do not write CREATE TABLE statements that re-create each dumped table	 
--no-defaults|Read no option files	 
--parallel-schemas|Specify schema-processing parallelism	 
--password|备份需要的密码|-p
--plugin-dir|Directory where plugins are installed	 
--port|备份数据库的TCP/IP端口	 
--print-defaults|Print default options	 
--protocol|指定连接服务器的协议	{TCP/SOCKET/PIPE/MEMORY}
--replace|备份出来replace into语句	 
--result-file|Direct output to a given file	 
--routines|备份出来包含存储过程和函数，默认开启，需要对 mysql.proc表有查看权限。生成的文件中会包含CREATE PROCEDURE 和 CREATE FUNCTION语句以用于恢复，关闭则需要用--skip-routines参数。	 
--secure-auth|Do not send passwords to server in old (pre-4.1) format	 
--set-charset|备份文件里写SET NAMES default_character_set 到输出，此参默认开启。 -- skip-set-charset禁用此参数，不会在备份文件里面写出set names...	 
--set-gtid-purged|Whether to add SET @@GLOBAL.GTID_PURGED to output	5.7.18
--single-transaction|该参数在事务隔离级别设置成Repeatable Read，并在dump之前发送start transaction 语句给服务端。这在使用innodb时很有用，因为在发出start transaction时，保证了在不阻塞任何应用下的一致性状态。对myisam和memory等非事务表，还是会改变状态的，当使用此参的时候要确保没有其他连接在使用ALTER TABLE、CREATE TABLE、DROP TABLE、RENAME TABLE、TRUNCATE TABLE等语句，否则会出现不正确的内容或则失败。--add-locks和此参互斥，在mysql5.7.11之前，--default-parallelism大于1的时候和此参也互斥，必须使用--default-parallelism=0。5.7.11之后解决了--single-transaction和--default-parallelism的互斥问题。	 
--skip-definer|忽略那些创建视图和存储过程用到的 DEFINER 和 SQL SECURITY 语句，恢复的时候，会使用默认值，否则会在还原的时候看到没有DEFINER定义时的账号而报错。
--skip-dump-rows|只备份表结构，不备份数据。注意：mysqldump支持--no-data，mysqlpump不支持--no-data |-d
--socket|对于连接到localhost，Unix使用套接字文件，在Windows上是命名管道的名称使用|-S	 
--ssl|Enable encrypted connection	 
--ssl-ca|File that contains list of trusted SSL Certificate Authorities	 
--ssl-capath|Directory that contains trusted SSL Certificate Authority certificate files	 
--ssl-cert|File that contains X509 certificate	 
--ssl-cipher|List of permitted ciphers for connection encryption	 
--ssl-crl|File that contains certificate revocation lists	 
--ssl-crlpath|Directory that contains certificate revocation list files	 
--ssl-key|File that contains X509 key	 
--ssl-mode|Security state of connection to server	5.7.11
--ssl-verify-server-cert|Verify host name against server certificate Common Name identity	 
--tls-version|Protocols permitted for encrypted connections	5.7.10
--triggers|备份出来包含触发器，默认开启，使用--skip-triggers来关闭。	 
--tz-utc|备份时会在备份文件的最前几行添加SET TIME_ZONE='+00:00'。注意：如果还原的服务器不在同一个时区并且还原表中的列有timestamp字段，会导致还原出来的结果不一致。默认开启该参数，用 --skip-tz-utc来关闭参数。
--user|备份时候的用户名|-u 
--users|备份数据库用户，备份的形式是CREATE USER...，GRANT...，只备份数据库账号可以通过如下命令	 
--version|Display version information and exit	5.7.9
--watch-progress|定期显示进度的完成，包括总数表、行和其他对象。该参数默认开启，用--skip-watch-progress来关闭。

### 三、备份列子
* 备份指定数据库的结构和数据（包含结构、自定义函数、触发器和存储过程）
```mysql
# mysql 5.7.11 或以上版本
mysqlpump -u root -p --add-drop-database --add-drop-table --triggers --routines --events --skip-definer lijiang_mq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/backup.sql

# mysql 5.7.11 以下版本
mysqldump -u root -p --opt --routines --triggers --events lijiang_mq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/backup.sql
```

* 备份数据库结构 （包含结构、自定义函数、触发器和存储过程）
```mysql
# mysql 5.7.11 或以上版本
mysqlpump  -u root -p --skip-dump-rows --add-drop-database --add-drop-table --triggers --routines --events --skip-definer --databases lijiang_mq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/tables_structure.sql

# mysql 5.7.11 以下版本
mysqldump  -u root -p --no-data --opt --add-drop-trigger --triggers --routines --events --databases lijiang_mq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/tables_structure.sql
```

* 仅备份数据
```mysql
# mysql 5.7.11 或以上版本
mysqlpump -u root -p --no-create-db --no-create-info --skip-triggers --skip-routines lijiang_mq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/data.sql

# mysql 5.7.11 以下版本
mysqldump -u root -p --quick --no-create-db --no-create-info --skip-triggers lijiang_mq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/data.sql
```

* 备份指定表的结构和数据（包含结构和触发器）
```mysql
# mysql 5.7.11 或以上版本
mysqlpump -u root -p --add-drop-table --triggers --skip-definer --databases lijiang_mq --include-tables=lijiang_rocketmq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/backup.sql

# mysql 5.7.11 以下版本
mysqldump -u root -p --opt --add-drop-trigger --triggers --databases lijiang_mq --tables=lijiang_rocketmq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/backup.sql
```

* 备份指定表的结构（包含结构和触发器）
```mysql
# mysql 5.7.11 或以上版本
mysqlpump  -u root -p --skip-dump-rows --add-drop-database --add-drop-table --triggers --skip-definer --databases lijiang_mq --include-tables=lijiang_rocketmq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/lijiang_rocketmq.sql

# mysql 5.7.11 以下版本
mysqldump  -u root -p --no-data --no-create-db --no-create-info --opt --add-drop-trigger --triggers --databases lijiang_mq --tables=lijiang_rocketmq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/lijiang_rocketmq.sql
```

* 仅备份指定表的数据
```mysql
# mysql 5.7.11 或以上版本
mysqlpump -u root -p --no-create-db --no-create-info --skip-triggers --skip-routines --databases lijiang_mq --include-tables=lijiang_rocketmq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/data.sql

# mysql 5.7.11 以下版本
mysqldump -u root -p --quick --no-create-db --no-create-info --skip-triggers --databases lijiang_mq --tables=lijiang_rocketmq > /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/data.sql
```

### 四、恢复
* 直接用 mysql 客户端
```mysql
mysql -uroot -p lijiang_mq < /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/data.sql
```

* 在 mysql 客户端中使用 SOURCE 命令
```mysql
mysql -uroot -p
mysql> SOURCE /Users/gadflybsd/PhpstormProjects/LiJiangCheLiangHuiJuTongJiXiTong/sql/data.sql