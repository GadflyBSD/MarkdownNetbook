* ## 配置 `PostgreSQL`
  * 配置 `PostgreSQL-MD5` 认证
    ```shell
    $ vi /data/pgsql/data/pg_hba.conf
    # "local" is for Unix domain socket connections only
    local        all      all                     md5
    # IPv4 local connections:
    host        all      all    192.168.1.0/24    md5
    host        all      all    127.0.0.1/32      md5
    # IPv6 local connections:
    host        all      all    ::1/128           md5
    ```
  * 配置 `PostgreSQL-Configure TCP/IP`
    ```shell
    $ vi /data/pgsql/data/postgresql.conf 
    [...]
    listen_addresses = '*’
    [...]
    port = 5432
    [...]
    ```
  * 修改 `postgresql` 时区
    > IntelliJ IDEA 中需要在`Data Sources and Drivers`中的当前链接里的`Advanced`中在`VM Options`里添加`-Duser.timezone=PRC`。
    ```shell
    $ vi /data/pgsql/data/postgresql.conf 
    [...]
    log_timezone = 'Asia/Shanghai’
    [...]
    timezone = 'Asia/Shanghai’
    [...]
    $ psql -U postgres -d postgres
    postgres=# show time zone;      # 查看检查PG系统时区
    ```
  * 修改 `postgresql` 区域
    ```shell
    $ vi /data/pgsql/data/postgresql.conf 
    [...]
    lc_messages = 'zh_CN.UTF-8'                     # 消息使用的语言
    lc_monetary = 'zh_CN.UTF-8'                     # 货币数量使用的格式
    lc_numeric = 'zh_CN.UTF-8'                      # 数字的格式
    lc_time = 'zh_CN.UTF-8'                         # 日期和时间的格式
    $ psql -U postgres -d postgres
    postgres=# SELECT '19.98':MONEY;
    ```
  * 建立安全的`SSL`方式连接`PostgreSQL`数据库服务器
    * 为了让SSL工作起来，需要添加下面三个文件到 $PGDATA服务器目录
      * `server.key` – 私钥
        ```shell
        openssl genrsa -des3 -out server.key 1024
        ```
        > 1. 在生成server.key文件的过程中，它会要求输入密码— 随意输入并确认。 
        > 2. 为了在以后使用这个key，你需要删除在上面添加的密码。
        
        ```shell
        openssl rsa -in server.key -out server.key
        chmod 400 server.key
        ```
      * `server.crt` – 服务器证书
        ```shell
        openssl req -new -key server.key -days 3650 -out server.crt -x509
        ```
        或者直接在命令行中指定
        ```shell
        openssl req -new -key server.key -days 3650 -out server.crt -x509 -subj ‘/C=CN/ST=YunNan/L=KunMing/O=www.zhongmengkeji.com/CN=postgres/ema ilAddress=postgres@zhongmengkeji.com’
        ```
        ![](https://img-blog.csdn.net/20170506050653407?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1NDY3NDU0OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
      * `root.crt` – 受信任的根证书
        ```shell
        cp server.crt root.crt
        ```
    * 配置`postgres.conf`和`pg_hba.conf`文件
      * 配置`postgres.conf`修改`ssl mode` 为`on`，设置`ssl_ca_file`为`root.crt` 
        ```shell
        sudo vi /data/pgsql/data/postgresql.conf 
        ssl = on
        ssl_ca_file = 'root.crt'
        ssl_cert_file = 'server.crt'
        ssl_key_file = 'server.key'
        ```
      * 修改`pg_hba.conf`文件，新增`ssl`认证连接的规则
        ```shell
        sudo vi /data/pgsql/data/pg_hba.conf 
        #host   all             all             0.0.0.0/0               md5
        hostssl all             all             0.0.0.0/0               md5
        ```
    * 重启数据库，`SSL`安全连接规则生效
      ```shell
      psql -U postgres -d postgres                # 本地连接没有使用SSL方式
      psql -h 127.0.0.1 -U postgres -d postgres   # TCP/IP 连接已经使用SSL方式
      ```
* ## 安装 `Hiredis`
```shell
git clone https://github.com/redis/hiredis.git
cd hiredis
make
mkdir /usr/lib/hiredis
cp libhiredis.so /usr/lib/hiredis
mkdir /usr/include/hiredis
cp hiredis.h /usr/include/hiredis
echo '/usr/local/lib' >>/etc/ld.so.conf 
ldconfig
```

* ## 安装 `redis_fdw`
```shell
git clone -b REL_10_STABLE https://github.com/pg-redis-fdw/redis_fdw.git
cd redis_fdw
export PATH=/usr/pgsql-11//bin:$PATH
make USE_PGXS=1
make USE_PGXS=1 install

su postgres
bash-4.2$ psql -U postgres -d postgres
postgres=# create extension redis_fdw;
CREATE EXTENSION

postgres=# CREATE SERVER redis_server
postgres-#     FOREIGN DATA WRAPPER redis_fdw
postgres-#     OPTIONS (address '127.0.0.1', port '6379');
CREATE SERVER

postgres=# CREATE USER MAPPING FOR PUBLIC
postgres-#     SERVER redis_server
postgres-#     OPTIONS (password '');
CREATE USER MAPPING
```

* ## redis_fdw详细的用法介绍
  1. CREATE SERVER 支持的 option(指定地址和端口)
      ```
      address: The address or hostname of the Redis server. Default: 127.0.0.1
      port: The port number on which the Redis server is listening. Default: 6379
      ```
  2. CREATE USER MAPPING 支持的 option (指定密码)
      ```
      password: The password to authenticate to the Redis server with. Default:
      ```
  3. CREATE FOREIGN TABLE 支持的 option
      * 指定数据库ID 
      * 表类型(hash,list,set,zset或scalar)
      * key 前缀 key 集合 singleton_key 指定KEY
      ```conf
      database: The numeric ID of the Redis database to query. Default: 0
      tabletype: can be 'hash', 'list', 'set' or 'zset' Default: none, meaning only look at scalar values.
      tablekeyprefix: only get items whose names start with the prefix Default: none
      tablekeyset: fetch item names from the named set Default: none
      singleton_key: get all the values in the table from a single named object. Default: none, meaning don't just use a single object.
      ```
      
* ## 安装`pg_bulkload`
  * ### pg_bulkload安装 
    ```shell
    su - postgres
    git clone https://github.com/ossc-db/pg_bulkload.git
    cd pg_bulkload/
    USE_PGXS=1 make all
    USE_PGXS=1 make install
    ```
    安装完成；要使用它需要建extension
    ```pgsql
    create extension pg_bulkload;
    ```
  * ### pg_bulkload参数
    ```shell
    [postgres@Postgres201 ~]$ pg_bulkload --help
    pg_bulkload is a bulk data loading tool for PostgreSQL
    Usage:
      Dataload: pg_bulkload [dataload options] control_file_path
      Recovery: pg_bulkload -r [-D DATADIR]
    Dataload options:
      -i, --input=INPUT         INPUT path or function    ## 从中加载数据的源。与控制文件中的“INPUT”相同。
      -O, --output=OUTPUT       OUTPUT path or table      ## 将数据加载到的目标
      -l, --logfile=LOGFILE     LOGFILE path              ## 写结果日志的路径
      -P, --parse-badfile=*     PARSE_BADFILE path        ## 用于写入无法正确解析的不良记录的路径。
      -u, --duplicate-badfile=* DUPLICATE_BADFILE path    ## 在索引重建期间写入与唯一约束冲突的坏记录的路径
      -o, --option="key=val"    additional option         ## 控制文件中可用的任何其它选项
    Recovery options:
      -r, --recovery            execute recovery
      -D, --pgdata=DATADIR      database directory
    Connection options:
      -d, --dbname=DBNAME       database to connect       ## 指定要连接的数据库的名称。
      -h, --host=HOSTNAME       database server host      ## 指定运行服务器的计算机的主机名。
      -p, --port=PORT           database server port      ## 定服务器在其上侦听连接的TCP端口
      -U, --username=USERNAME   user name to connect as   ## 要连接的用户名称
      -w, --no-password         never prompt for password
      -W, --password            force password prompt
    Generic options:
      -e, --echo                echo queries                            ## Echo命令发送到服务器
      -E, --elevel=LEVEL        set output message level                ## 从DEBUG，INFO，NOTICE，WARNING，ERROR，LOG，FATAL和PANIC中选择输出消息等级。默认值为INFO。
      --help                    show this help, then exit
      --version                 output version information, then exit
      ```
  * ### pg_bulkload的使用
    1. #### 不使用控制文件使用参数 
      ```shell
      [postgres@Postgres201 ~]$ pg_bulkload -i /home/postgres/tbl_lottu_output.txt -O tbl_lottu -l /home/postgres/tbl_lottu_output.log -P /home/postgres/tbl_lottu_bad.txt  -o "TYPE=CSV" -o "DELIMITER=|" -d lottu -U lottu
      
      NOTICE: BULK LOAD START
      NOTICE: BULK LOAD END
       0 Rows skipped.
       100000 Rows successfully loaded.
       0 Rows not loaded due to parse errors.
       0 Rows not loaded due to duplicate errors.
       0 Rows replaced with new rows.
       
      [postgres@Postgres201 ~]$ cat tbl_lottu_output.log
      
      pg_bulkload 3.1.9 on 2018-07-12 13:37:18.326685+08
      INPUT = /home/postgres/tbl_lottu_output.txt
      PARSE_BADFILE = /home/postgres/tbl_lottu_bad.txt
      LOGFILE = /home/postgres/tbl_lottu_output.log
      LIMIT = INFINITE
      PARSE_ERRORS = 0
      CHECK_CONSTRAINTS = NO
      TYPE = CSV
      SKIP = 0
      DELIMITER = |
      QUOTE = "\""
      ESCAPE = "\""
      NULL = 
      OUTPUT = lottu.tbl_lottu
      MULTI_PROCESS = NO
      VERBOSE = NO
      WRITER = DIRECT
      DUPLICATE_BADFILE = /data/postgres/data/pg_bulkload/20180712133718_lottu_lottu_tbl_lottu.dup.csv
      DUPLICATE_ERRORS = 0
      ON_DUPLICATE_KEEP = NEW
      TRUNCATE = NO
        0 Rows skipped.
        100000 Rows successfully loaded.
        0 Rows not loaded due to parse errors.
        0 Rows not loaded due to duplicate errors.
        0 Rows replaced with new rows.
      Run began on 2018-07-12 13:37:18.326685+08
      Run ended on 2018-07-12 13:37:18.594494+08
      CPU 0.14s/0.07u sec elapsed 0.27 sec
      ```
    2. #### 导入之前先清理表数据
    ```shell
    [postgres@Postgres201 ~]$ pg_bulkload -i /home/postgres/tbl_lottu_output.txt -O tbl_lottu -l /home/postgres/tbl_lottu_output.log -P /home/postgres/tbl_lottu_bad.txt  -o "TYPE=CSV" -o "DELIMITER=|" -o "TRUNCATE=YES" -d lottu -U lottu
    NOTICE: BULK LOAD START
    NOTICE: BULK LOAD END
     0 Rows skipped.
     100000 Rows successfully loaded.
     0 Rows not loaded due to parse errors.
     0 Rows not loaded due to duplicate errors.
     0 Rows replaced with new rows.
     ```
    3. #### 使用控制文件
       > 新建控制文件lottu.ctl
       ```shell
       INPUT = /home/postgres/lotu01
       PARSE_BADFILE = /home/postgres/tbl_lottu_bad.txt
       LOGFILE = /home/postgres/tbl_lottu_output.log
       ## 要加载的行数。默认值为INFINITE，即所有数据将被加载。
       LIMIT = INFINITE
       ## 无效的输入元组未加载并记录在PARSE BADFILE中。默认值为0.如果有等于或大于该值的解析错误，则已提交已加载的数据，并且不加载剩余的元组。 0表示不允许错误，-1和INFINITE表示忽略所有错误。
       PARSE_ERRORS = 0
       ## 指定在加载期间是否检查CHECK约束。默认值为NO。
       CHECK_CONSTRAINTS = NO
       TYPE = CSV
       ## 跳过输入行的数量。默认值为0.您不能同时指定“TYPE = FUNCTION”和SKIP。
       SKIP = 5
       ## delimiter_character 单个ASCII字符，用于分隔文件每行（行）中的列。默认值为逗号。
       DELIMITER = |
       ## quote_character 指定ASCII引号字符。默认值为双引号
       QUOTE = "\""
       ##  escape_character 指定应在QUOTE数据字符值之前出现的ASCII字符。默认值为双引号
       ESCAPE = "\""
       OUTPUT = lottu.tbl_lottu
       ## 如果是，我们使用多线程并行地进行数据读取，解析和写入。如果否，我们只使用单线程，而不是做并行处理。默认值为NO。
       MULTI_PROCESS = NO
       ## 加载数据的方法。默认值为DIRECT。
       WRITER = DIRECT
       DUPLICATE_BADFILE = /home/postgres/tbl_lottu.dup.csv
       ## 违反唯一约束的导入元组数。冲突的元组从表中删除并记录在DUPLICATE BADFILE中。默认值为0.如果存在等于或大于该值的唯一违例，则回滚整个负载。 0表示不允许违反，-1和INFINITE表示忽略所有违规。
       DUPLICATE_ERRORS = 0
       ON_DUPLICATE_KEEP = NEW
       ## 如果为YES，使用TRUNCATE命令从目标表中删除所有行。如果否，不做任何事。默认值为NO。
       TRUNCATE = YES
       ```
       > 使用控制文件进行加载操作
       ```shell
       [postgres@Postgres201 ~]$ pg_bulkload  /home/postgres/lottu.ctl -d lottu -U lottu
       
        NOTICE: BULK LOAD START
        NOTICE: BULK LOAD END
         5 Rows skipped.
         95 Rows successfully loaded.
         0 Rows not loaded due to parse errors.
         0 Rows not loaded due to duplicate errors.
         0 Rows replaced with new rows.
       ```
      4. #### Option 参数选项说明
          * WRITER | LOADER 加载数据的方法。
    
          参数|说明
          -|-
          DIRECT|直接将数据加载到表。跳过共享缓冲区并跳过WAL日志记录，但需要自己的恢复过程。这是默认值和原始旧版本的模式。
          BUFFERED|通过共享缓冲区将数据加载到表。使用共享缓冲区，写入WAL，并使用原始的PostgreSQL WAL恢复。
          BINARY|将数据转换为二进制文件，可以用作要从中加载的输入文件。创建加载输出二进制文件所需的控制文件示例。此示例文件在与二进制文件相同的目录中创建，其名称为<binary-file-name> .ctl。
          PARALLEL|与“WRITER = DIRECT”和“MULTI_PROCESS = YES”相同。如果指定了PARALLEL，则忽略MULTI_PROCESS。如果将密码认证配置为要加载的数据库，则必须设置密码文件。
          
          * COL = type 二进制输入格式
  * ### 
  