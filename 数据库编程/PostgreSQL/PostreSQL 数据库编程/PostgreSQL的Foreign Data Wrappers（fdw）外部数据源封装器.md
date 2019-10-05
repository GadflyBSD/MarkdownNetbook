* ## `SQL`关系型数据库的跨库操作
  * ### `PostgreSQL`关系型数据库
    1. #### 安装扩展
        ```pgsql
        create extension postgres_fdw;
        ```
    2. #### 创建`server`
        ```pgsql
        create server server_remote_farr foreign data wrapper postgres_fdw 
          options(host '172.16.3.59',port '1921',dbname 'postgres');
        ```
    3. #### 创建`user mapping`
        ```pgsql
        create user mapping for android_market server server_remote_farr 
          options(user 'skydata_test',password 'skydata_test');
        ```
    4. 创建`PostgreSQL`外部数据表
      ```pgsql
      CREATE FOREIGN TABLE tbl_kenyon(
        id int,
        create_user int,
        model_name varchar,
        flag int
      )server server_remote_farr 
        options (schema_name 'metadata',table_name 'guide_model');
      ```

  * ### `MySQL`关系型数据库
    1. #### 安装扩展
        * 下载扩展
          ```shell
          https://github.com/EnterpriseDB/mysql_fdw
          ```
        * 编译安装
          ```shell
          git clone https://github.com/EnterpriseDB/mysql_fdw
          cd mysql_fdw
          export PATH=/usr/local/Cellar/postgresql/11.2//bin:$PATH
          export PATH=/usr/local/mysql/bin/:$PATH
          export LD_LIBRARY_PATH=/usr/local/mysql/lib
          make USE_PGXS=1
          make USE_PGXS=1 install
          sudo ln -s /usr/local/mysql/lib/libmysqlclient.dylib /usr/lib/libmysqlclient.dylib
          ```
        * 在`SQL`命令行中安装扩展
          ```pgsql
          create extension mysql_fdw;
          ```
    2. #### 创建`server`
        ```pgsql
        CREATE SERVER mysql_svr  
          FOREIGN DATA WRAPPER mysql_fdw  
          OPTIONS (host '127.0.0.1', port '3306');
        ```
    3. #### 创建`user mapping`
        ```pgsql
        CREATE USER MAPPING FOR postgres SERVER mysql_svr  
          OPTIONS (username 'root', password '123456');
        ```
    4. #### 创建`MySQL`外部数据表
        ```pgsql
        CREATE FOREIGN TABLE pg_mysql_tbl_fdw (id integer,vname text)
          SERVER mysql_svr OPTIONS (dbname 'mysql_fdw',table_name 'tbl_fdw');
        ```

  * ### `Oracle`关系型数据库
    1. #### 安装扩展
      * 下载扩展
        ```shell
        https://github.com/laurenz/oracle_fdw
        ```
      * 在`SQL`命令行中安装扩展
        ```pgsql
        create extension oracle_fdw;
        ```
    2. #### 创建`server`
        ```pgsql
        CREATE SERVER oradb FOREIGN DATA WRAPPER oracle_fdw
              OPTIONS (dbserver '//dbserver.mydomain.com/ORADB');
              ////dbserver.mydomain.com/ORADB指的是tns的名称
        GRANT USAGE ON FOREIGN SERVER oradb TO pguser;
        ```
    3. #### 创建`user mapping`
        ```pgsql
        CREATE USER MAPPING FOR pguser SERVER oradb  
          OPTIONS (user 'orauser', password 'orapwd');
        ```
    4. #### 创建`Oracle`外部数据表
        ```pgsql
        CREATE FOREIGN TABLE oratab (
            id        integer OPTIONS (key 'true')  NOT NULL,
            text      character varying(30),
            floating  double precision  NOT NULL
        ) SERVER oradb OPTIONS (schema 'ORAUSER', table 'ORATAB');
        ```
  
  * ### `SqlServer`关系型数据库
    1. #### 安装扩展
      * 下载扩展
        ```shell
        https://github.com/tds-fdw/tds_fdw
        ```
      * 在`SQL`命令行中安装扩展
        ```pgsql
        create extension tds_fdw;
        ```
    2. #### 创建`server`
        ```pgsql
        CREATE SERVER mssql_svr FOREIGN DATA WRAPPER tds_fdw
          OPTIONS (servername '127.0.0.1', port '1433', database 'tds_fdw_test');
        ```
    3. #### 创建`user mapping`
        ```pgsql
        CREATE USER MAPPING FOR postgres SERVER mssql_svr 
          OPTIONS (username 'sa', password 'xxxx');
        ```
    4. #### 创建`SqlServer`外部数据表
        ```pgsql
        CREATE FOREIGN TABLE mssql_table (
            id integer,
            data varchar)
            SERVER mssql_svr
            OPTIONS (table_name 'dbo.mytable');
        ```
      
* ## `Hadoop`大数据
  1. #### 安装扩展
    * 下载扩展
      ```shell
      https://github.com/EnterpriseDB/hdfs_fdw
      ```
    * 在`SQL`命令行中安装扩展
      ```pgsql
      CREATE EXTENSION hdfs_fdw;
      ```
  2. #### 创建`server`
      ```pgsql
      CREATE SERVER hdfs_svr FOREIGN DATA WRAPPER hdfs_fdw
        OPTIONS (host '127.0.0.1',port '10000',client_type 'spark');
      ```
  3. #### 创建`user mapping`
      ```pgsql
      CREATE USER MAPPING FOR postgres server hdfs_svr 
        OPTIONS (username 'ldapadm', password 'ldapadm');
      ```
  4. #### 创建`Hadoop`外部数据表
      ```pgsql
      CREATE FOREIGN TABLE f_names_tab( 
        a int, 
        name varchar(255)
      ) SERVER hdfs_svr
        OPTIONS (dbname 'testdb', table_name 'my_names_tab');
      ```
      
* ## `MongoDB` `NoSQL`数据库
  1. #### 安装扩展
    * 下载扩展
      ```shell
      https://github.com/EnterpriseDB/mongo_fdw
      ```
    * 在`SQL`命令行中安装扩展
      ```pgsql
      CREATE EXTENSION mongo_fdw;
      ```
  2. #### 创建`server`
      ```pgsql
      CREATE SERVER mongo_server
         FOREIGN DATA WRAPPER mongo_fdw
         OPTIONS (address '127.0.0.1', port '27017');
      ```
  3. #### 创建`user mapping`
      ```pgsql
      CREATE USER MAPPING FOR postgres
        SERVER mongo_server
        OPTIONS (username 'mongo_user', password 'mongo_pass');
      ```
  4. #### 创建`MongoDB`外部数据表
      ```pgsql
      CREATE FOREIGN TABLE warehouse(
        _id NAME,
        warehouse_id int,
        warehouse_name text,
        warehouse_created timestamptz
      )SERVER mongo_server
         OPTIONS (database 'db', collection 'warehouse');
      ```
* ## `Redis`
  * ### 安装 `Hiredis`
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
  * ### 安装 `redis_fdw`
    * #### 下载扩展
      ```shell
      git clone -b REL_10_STABLE https://github.com/pg-redis-fdw/redis_fdw.git
      cd redis_fdw
      export PATH=/usr/local/Cellar/postgresql/11.2//bin:$PATH
      make USE_PGXS=1
      make USE_PGXS=1 install
      ```
    * #### 在`SQL`命令行中安装扩展
      ```pgsql
      create extension redis_fdw;
      ```
  * ### 创建`server`
    ```pgsql
    CREATE SERVER redis_server
        FOREIGN DATA WRAPPER redis_fdw
        OPTIONS (address '127.0.0.1', port '6379');
    ```
  * ### 创建`user mapping`
    ```pgsql
    CREATE USER MAPPING FOR PUBLIC
        SERVER redis_server
        OPTIONS (password '');
    ```
  * ### 创建`Redis`外部数据表
    ```pgsql
    CREATE FOREIGN TABLE redis_localhost_15 (
      "key" text, 
      "val" json
    )SERVER redis_server OPTIONS (database '15');
    ```
  * ## redis_fdw详细的用法介绍
    1. CREATE SERVER 支持的 option(指定地址和端口)
        ```yml
        address: The address or hostname of the Redis server. Default is 127.0.0.1
        port: The port number on which the Redis server is listening. Default: 6379
        ```
    2. CREATE USER MAPPING 支持的 option (指定密码)
        ```yml
        password: The password to authenticate to the Redis server with. Default is ''
        ```
    3. CREATE FOREIGN TABLE 支持的 option
        * 指定数据库ID 
        * 表类型(hash,list,set,zset或scalar)
        * key 前缀 key 集合 singleton_key 指定KEY
        ```yml
        database: The numeric ID of the Redis database to query. Default is 0
        tabletype: can be 'hash', 'list', 'set' or 'zset' Default: none, meaning only look at scalar values.
        tablekeyprefix: only get items whose names start with the prefix Default is none
        tablekeyset: fetch item names from the named set Default is none
        singleton_key: get all the values in the table from a single named object. Default is none, meaning don't just use a single object.
        ```
* ## `Kafka`
   1. #### 安装扩展
    * 下载扩展
      ```shell
      https://github.com/adjust/kafka_fdw
      ```
    * 在`SQL`命令行中安装扩展
      ```pgsql
      CREATE EXTENSION kafka_fdw;
      ```
  2. #### 创建`server`
      ```pgsql
      CREATE SERVER kafka_server
        FOREIGN DATA WRAPPER kafka_fdw
        OPTIONS (brokers 'localhost:9092');
      ```
  3. #### 创建`user mapping`
      ```pgsql
      CREATE USER MAPPING FOR PUBLIC SERVER kafka_server;
      ```
  4. #### 创建`MongoDB`外部数据表
      ```pgsql
      CREATE FOREIGN TABLE kafka_test (
        part int OPTIONS (partition 'true'),
        offs bigint OPTIONS (offset 'true'),
        some_int int,
        some_text text,
        some_date date,
        some_time timestamp
      )SERVER kafka_server OPTIONS(
        format 'csv', 
        topic 'contrib_regress', 
        batch_size '30', 
        buffer_delay '100'
      );
      ```