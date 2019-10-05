* ## 外部文件操作 `file_fdw`
  > 外部数据包装器,以只读方式打开并输出服务器的文件系统中的数据文件
  * ### 安装扩展（系统自带扩展，不需要额外编译安装）
    ```pgsql
    CREATE EXTENSION IF NOT EXISTS file_fdw;
    ```
  * ### 创建外部服务器
    ```pgsql
    CREATE SERVER pgfile FOREIGN DATA WRAPPER file_fdw;
    ```
  * ### 创建外部数据表
    ```pgsql
    CREATE FOREIGN TABLE provinces_csv(
      code integer,
      name varchar(100)
    ) SERVER pgcvs
	  OPTIONS ( 
	    filename '/Users/gadflybsd/Downloads/provinces.csv', 
	    format 'csv',
	    header 'true'
	  );
    ```
    > 创建一个外部数据表的时候可以有下列选项：
    
    参数名|参数说明
    -|-
    filename|指定要被读取的文件。必须是一个绝对路径名。 必须指定filename或program， 但不能同时指定两个。
    program|指定要执行的命令。该命令的标准输出将被读取， 就像使用COPY FROM PROGRAM一样。必须指定program 或filename，但不能同时指定两个。
    format|指定数据的格式。比如`cvs`、`log`、`text`等
    header|指定数据是否具有一个头部行。
    delimiter|指定数据的定界符字符。
    quote|指定数据的引用字符。
    escape|指定数据的转义字符。
    null|指定数据的空字符串。
    encoding|指定数据的编码。
    
  * ### 用户标准`SQL`语言对外部数据表进行操作
    ```pgsql
    select * from public.provinces_csv;
    select * from public.provinces_csv where code = '53';
    ```
* ## `API`数据请求操作 `http_fdw`
  * ### 下载安装扩展
    * #### 下载地址 
      ```
      https://github.com/pramsey/pgsql-http
      ```
    * #### 安装扩展
      ```pgsql
      CREATE EXTENSION IF NOT EXISTS http;
      ```
  * ### 通过SQL语言发起一个GET方式的API请求
    ```pgsql
    SELECT json_extract_path_text(content::json, 'uuid') AS "uuid" FROM http_get('http://httpbin.org/uuid');
    ```
  * ### 通过SQL语言发起一个PUT方式的API请求
    ```pgsql
    SELECT status, content_type, content::json->>'data' AS data
    FROM http_put('http://httpbin.org/put', '我是谁？', 'text/plain');
    ```
  * ### 通过SQL语言发起一个POST方式的API请求
    ```pgsql
    SELECT status, content::json->>'form' AS myvar
    FROM http_post('http://httpbin.org/post',
               'myvar=' || urlencode('my special string & things?'),
               'application/x-www-form-urlencoded');
    ```
  * ### 扩展所提供的函数说明
    * `http_header(field VARCHAR, value VARCHAR)` returns `http_header`
    * `http(request http_request)` returns `http_response`
    * `http_get(uri VARCHAR)` returns `http_response`
    * `http_post(uri VARCHAR, content VARCHAR, content_type VARCHAR)` returns `http_response`
    * `http_put(uri VARCHAR, content VARCHAR, content_type VARCHAR)` returns `http_response`
    * `http_patch(uri VARCHAR, content VARCHAR, content_type VARCHAR)` returns `http_response`
    * `http_delete(uri VARCHAR)` returns `http_response`
    * `http_head(uri VARCHAR)` returns `http_response`
    * `http_set_curlopt(curlopt VARCHAR, value varchar)` returns `boolean`
    * `http_reset_curlopt()` returns `boolean`
    * `urlencode(string VARCHAR)` returns `text` 
* ## `SQL`数据库跨库ETL操作 `mysql_fdw`
   1. ### 安装扩展
      * #### 下载扩展
          ```shell
          https://github.com/EnterpriseDB/mysql_fdw
          ```
      * #### 编译安装
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
      * #### 在`SQL`命令行中安装扩展
        ```pgsql
        CREATE EXTENSION IF NOT EXISTS mysql_fdw;
        ```
  2. ### 创建`server`
      ```pgsql
      CREATE SERVER mysql_server
      	FOREIGN DATA WRAPPER mysql_fdw
      	OPTIONS (host 'localhost', port '3306');
      ```
  3. ### 创建`user mapping`
      ```pgsql
      CREATE USER MAPPING FOR postgres SERVER mysql_server
        OPTIONS (username 'psr', password 'psr');
      ```
  4. ### 创建`MySQL`外部数据表
      ```pgsql
      CREATE FOREIGN TABLE mysql_psr_org (
        id varchar(255),
        name varchar(255),
        parent_id varchar(255)
      )SERVER mysql_server OPTIONS (dbname 'psr',table_name 'org');
      ```
  5. ### 用户标准`SQL`语言对外部数据表进行操作
      * #### 向`MySQL`数据库中添加一条记录
        ```pgsql
        INSERT INTO mysql_psr_org VALUES('530000000001', '云南省公安厅治安总队', '530000000000');
        ```
      * #### 通过`SELECT`语句查询刚刚添加的数据记录
        ```pgsql
        SELECT * FROM mysql_psr_org WHERE id='530000000001';
        ```
      * #### 通过`DELETE`语句删除刚刚添加的数据记录
        ```pgsql
        DELETE FROM mysql_psr_org WHERE id='530000000001';
        ```
* ## `NoSQL`数据服务操作 `redis_fdw`
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
  * ### redis_fdw详细的用法介绍
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
  * ### 测试
    * #### 读取Redis服务器中的数据
      ```pgsql
      select * from redis_localhost_15;
      ```
    * #### 向Redis服务器中新增一条数据
      ```pgsql
      insert into redis_localhost_15 (key, val) values ('test', '{"key": "test"}');
      ```
    * #### 将Redis服务器中刚刚添加的记录删除
      ```pgsql
      delete from redis_localhost_15 where key='test';
      ```
* ## `PostgreSQL`的`RestFUL API`服务
  > PostgREST 是一个独立的 Web 服务器，可为您的 PostgreSQL 数据库直接生成 RESTful API。 数据库中的结构约束和权限决定了 API 的端点（endpoints）和操作。
  * ### 下载地址：`https://github.com/PostgREST/postgrest/releases`
  * ### 开发文档：`http://postgrest.org/en/v5.2/index.html`
  * ### 创建配置文件`postgrst.conf`
    ```
    # postgrest.conf

    # The standard connection URI format, documented at
    # https://www.postgresql.org/docs/current/static/libpq-connect.html#AEN45347
    db-uri       = "postgres://postgres:postgres@127.0.0.1:5432/postgres"
    
    # The name of which database schema to expose to REST clients
    db-schema    = "public"
    
    # The database role to use when no client authentication is provided.
    # Can (and probably should) differ from user in db-uri
    db-anon-role = "postgres"
    
    server-host = "0.0.0.0"
    server-port = 4000
    ```
  * ### 运行`PostREST`
    ```shell
    ./postgrest postgrest.conf
    ```
  * ### 测试`PostgreSQL RestFUL API`
    > 使用浏览器或者`PostMan`测试`API`接口

  * ### postgREST 角色授权
      [postgREST 角色授权](http://postgrest.org/en/v5.1/tutorials/tut0.html)
  * ### 结合JWT实现权限验证
      [结合JWT实现权限验证](http://postgrest.org/en/v5.1/tutorials/tut1.html#tutorial-1-the-golden-key)
      [PostgREST实现用户权限安全分级](https://blog.csdn.net/qq_41647999/article/details/84941111)
      [postgREST 角色授权文档整理](https://blog.csdn.net/henjuewang/article/details/81011968)