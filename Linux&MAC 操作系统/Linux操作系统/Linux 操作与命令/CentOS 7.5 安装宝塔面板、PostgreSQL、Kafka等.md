### 1、安装之前先检查一下系统是否有默认安装的`apache`或者`php`
```shell
$ rpm -qa|grep httpd
$ rpm -qa|grep php
$ rpm -qa|grep mysql
```

### 2、把上面指令列出来的包删除
```shell
$ rpm -e ****(包名)
```

### 3、修改`CentOS`默认`yum`源为国内`yum`镜像源
1. 备份`/etc/yum.repos.d/CentOS-Base.repo`
    ```shell
    mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
    ```
2. 下载`163`的`yum`源配置文件到上面那个文件夹内
    ```shell
    wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
    ```
3. 运行`yum makecache`生成缓存
    ```shell
    yum makecache
    ```
4. 更新系统
    ```shell
    yum -y update
    ```
    
### 4、安装一些必备的包
```shell
yum -y install gcc gcc-c++ make screen wget 
screen -S bt
```

### 5、编译安装`hiredis`
  * `hiredis`下载地址 [releases](https://github.com/redis/hiredis/releases)
  * 编译安装
    ```shell
    git clone https://github.com/redis/hiredis.git
    cd hiredis
    make
    make install
    mkdir /usr/lib/hiredis
    cp libhiredis.so /usr/lib/hiredis
    mkdir /usr/include/hiredis
    cp hiredis.h /usr/include/hiredis
    echo '/usr/local/lib' >>/etc/ld.so.conf 
    ldconfig
    ```
    
### 6. 将已经挂载在 `home` 目录上的硬盘挂载到 `www` 目录上
```shell
$ df -h                   #（查看分区情况及数据盘名称）
$ mkdir /www              #（如果没有data目录就创建，否则此步跳过）
$ umount /home            #（卸载硬盘已挂载的home目录）
$ mount /dev/mapper/centos-home /www    #（挂载到data目录）
$ vi /etc/fstab           #（编辑fstab文件修改或添加，使重启后可以自动挂载）
mount /dev/mapper/centos-home /www ext3 auto 0 0
```

### 6、安装宝塔面板
```shell
wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
```

### 7、安装`MySQL 5.7`的`UDF`函数
* 为`MySQL`增加`HTTP/REST`插件：`MySQL UDF` 函数 `mysql-udf-http`
  1. 在`Linux`系统上安装`Mysql-udf-http`
      ```shell
      $ wget http://curl.haxx.se/download/curl-7.57.0.tar.gz
      $ tar zxvf curl-7.57.0.tar.gz
      $ cd curl-7.57.0/
      $ ./configure --prefix=/usr
      $ make && make install
      $ cd ../
      $ export PKG_CONFIG=/usr/bin/pkg-config
      $ export PKG_CONFIG_PATH=/usr/share/pkgconfig:/usr/lib/pkgconfig
      $ echo “/www/server/mysql/lib" > /etc/ld.so.conf.d/mysql.conf
      $ /sbin/ldconfig
      $ git clone https://github.com/y-ken/mysql-udf-http.git
      $ cd mysql-udf-http
      $ chmod +x ./configure
      $ ./configure --with-mysql=/www/server/mysql/bin/mysql_config --prefix=/usr --libdir=/www/server/mysql/lib/plugin
      $ make && make install
      $ cd ../
      ```
  2. 通过命令行登陆进入`MySQL`
      ```shell
      $ mysql -u root -p
      ```
  3. 创建 `MySQL` 自定义函数
      ```shell
      mysql> DROP FUNCTION IF EXISTS http_get; 
      mysql> DROP FUNCTION IF EXISTS http_post; 
      mysql> DROP FUNCTION IF EXISTS http_put; 
      mysql> DROP FUNCTION IF EXISTS http_delete; 
      mysql> create function http_get returns string soname 'mysql-udf-http.so';
      mysql> create function http_post returns string soname 'mysql-udf-http.so'; 
      mysql> create function http_put returns string soname 'mysql-udf-http.so'; 
      mysql> create function http_delete returns string soname 'mysql-udf-http.so';
      ```
* 为 `MySQL` 增加 `Lib_MySQLudf_sys` 自定义函数调用外部命令
  1. 在`Linux` 系统上安装 `Mysql-udf-sys`
      ```shell
      $ git clone https://github.com/mysqludf/lib_mysqludf_sys.git
      $ cd lib_mysqludf_sys
      $ gcc -DMYSQL_DYNAMIC_PLUGIN -fPIC -Wall -I/www/server/mysql/include -I. -shared lib_mysqludf_sys.c -o lib_mysqludf_sys.so
      $ cp lib_mysqludf_sys.so /www/server/mysql/lib/plugin
      ```
  2. 通过命令行登陆进入 `MySQL`
      ```shell
      $ mysql -u root -p
      ```
  3. 创建 `MySQL` 自定义函数
      ```shell
      mysql> DROP FUNCTION IF EXISTS lib_mysqludf_sys_info; 
      mysql> DROP FUNCTION IF EXISTS lib_mysqludf_sys_info;
      mysql> DROP FUNCTION IF EXISTS sys_set;
      mysql> DROP FUNCTION IF EXISTS sys_exec;
      mysql> DROP FUNCTION IF EXISTS sys_eval;
      mysql> CREATE FUNCTION lib_mysqludf_sys_info RETURNS string SONAME 'lib_mysqludf_sys.so';
      mysql> CREATE FUNCTION sys_get RETURNS string SONAME 'lib_mysqludf_sys.so';
      mysql> CREATE FUNCTION sys_set RETURNS int SONAME 'lib_mysqludf_sys.so';
      mysql> CREATE FUNCTION sys_exec RETURNS int SONAME 'lib_mysqludf_sys.so';
      mysql> CREATE FUNCTION sys_eval RETURNS string SONAME 'lib_mysqludf_sys.so';
      ```

### 8、为 `PHP` 添加额外扩展
  1. 为 `PHP` 添加 `PostgreSQL` 扩展
      ```SHELL
      $ cd /www/server/php/72/src/ext/pgsql
      $ /www/server/php/72/bin/phpize
      $ ./configure --with-php-config=/www/server/php/72/bin/php-config --with-pgsql=/usr/pgsql-10/
      $ make && make install
      ```
  2. 为 `PHP` 添加 `PDO-PostgreSQL` 扩展
      ```SHELL
      $ cd /www/server/php/72/src/ext/pdo_pgsql
      $ /www/server/php/72/bin/phpize
      $ ./configure --with-php-config=/www/server/php/72/bin/php-config --with-pdo-pgsql=/usr/pgsql-10/
      $ make && make install
      ```
      > 如果正常，会发现/www/server/php/72/lib/php/extensions/no-debug-non-zts-20170718/这里会多两个pdo_pgsql.so 和 pgsql.so
    
  3. 为 `PHP` 添加 `zookeeper` 扩展
      * 为 `PHP` 添加 `libzookeeper` 扩展
        ```shell
        $ git clone https://github.com/Timandes/libzookeeper.git
        $ cd libzookeeper
        $ /www/server/php/72/bin/phpize
        $ ./configure --with-php-config=/www/server/php/72/bin/php-config --with-libzookeeper=/usr/local/bin/cli_mt
        $ make && make install
        ```
      * 为 `PHP` 添加 `zookeeper` 扩展
        ```shell
        $ git clone -b php7 https://github.com/jbboehr/php-zookeeper.git
        $ cd /www/server/php-zookeeper
        $ /www/server/php/72/bin/phpize
        $ ./configure --with-php-config=/www/server/php/72/bin/php-config
        $ make && make install
        ```

### 9、安装 `PostgreSQL 10`
1. 根据你的服务器架构添加 `PostgreSQL` 库：
    ```shell
    $ rpm -Uvh https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm
    ```
2. 使用以下命令来更新库
    ```shell
    $ yum update
    ```
3. 使用以下命令来安装 `PostgreSQL`
    ```shell
    $ yum install postgresql10-server postgresql10-contrib postgresql10-devel
    ```
4. 使用以下命令来初始化 `PostgreSQL` 数据库
    ```shell
    $ mkdir /www/server/pgsql
    $ chown -R postgres:postgres /www/server/pgsql
    $ su postgres
    $ /usr/pgsql-10/bin/initdb -D /www/server/pgsql/10/data
    $ vi /usr/lib/systemd/system/postgresql-10.service
    Environment=PGDATA=/www/server/pgsql/10/data
    $ l
    ```
6. 配置默认 `postgres` 用户及密码
    ```shell
    $ su – postgres
    $ psql
    psql (10.2)
    Type "help" for help.
    Postgres=# 
    postgres=# \password postgres
    Enter new password:
    Enter it again:
    postgres=# CREATE EXTENSION adminpack;
    CREATE EXTENSION 
    postgres=# \q
    ```
7. 创建新的用户和数据库
    ```shell
    $ su – postgres
    $ createuser gadfly
    $ createdb gadfly_db
    $ psql
    psql (9.3.5)
    Type "help" for help. 
    postgres=# alter user gadfly with encrypted password 'centos';
    ALTER ROLE 
    postgres=# grant all privileges on database gadfly_db to gadfly;
    GRANT
    postgres=# 
    ```
8. 配置 `PostgreSQL-MD5` 认证
    ```shell
    $ vi /www/server/pgsql/10/data/pg_hba.conf
    # "local" is for Unix domain socket connections only
    local        all      all                     md5
    # IPv4 local connections:
    host        all      all    127.0.0.1/32      md5
    host        all      all    192.168.1.0/24    md5
    # IPv6 local connections:
    host        all      all    ::1/128           md5
    ```
9. 配置 `PostgreSQL-Configure TCP/IP`
    ```shell
    $ vi /www/server/pgsql/10/data/postgresql.conf 
    [...]
    listen_addresses = '*’
    [...]
    port = 5432
    [...]
    ```
10. 重启 `postgresql` 服务以应用更改
    ```shell
    systemctl restart postgresql-10
    ```
11. 更改数据库路径
    ```shell
    $ systemctl stop postgresql-10
    $ mkdir /www/server/pgsql
    $ cp -rf /var/lib/pgsql/10/ /www/server/pgsql/10
    $ chown -R postgres:postgres /www/server/pgsql
    $ chmod 700 /www/server/pgsql
    $ vi /usr/lib/systemd/system/postgresql-10.service
    Environment=PGDATA=/www/server/pgsql/10/data
    $ systemctl daemon-reload
    $ systemctl start postgresql-10
    ```
12. 安装 `redis_fdw`
    ```shell
    git clone -b REL_10_STABLE https://github.com/pg-redis-fdw/redis_fdw.git
    cd redis_fdw
    export PATH=/usr/pgsql-10//bin:$PATH
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

    > `redis_fdw` 详细的用法介绍
      1. `CREATE SERVER` 支持的 `option`(指定地址和端口)
          ```
          address: The address or hostname of the Redis server. Default: 127.0.0.1
          port: The port number on which the Redis server is listening. Default: 6379
          ```
      2. `CREATE USER MAPPING` 支持的 `option` (指定密码)
          ```
          password: The password to authenticate to the Redis server with. Default:
          ```
      3. `CREATE FOREIGN TABLE` 支持的 `option`
          * 指定数据库ID 
          * 表类型(`hash`,`list`,`set`,`zset`或`scalar`)
          * `key` 前缀 `key` 集合 `singleton_key` 指定 `KEY`
          ```conf
          database: The numeric ID of the Redis database to query. Default: 0
          tabletype: can be 'hash', 'list', 'set' or 'zset' Default: none, meaning only look at scalar values.
          tablekeyprefix: only get items whose names start with the prefix Default: none
          tablekeyset: fetch item names from the named set Default: none
          singleton_key: get all the values in the table from a single named object. Default: none, meaning don't just use a single object.
          ```
          
### 10、安装 `kafka` 和 `RabbitMQ`
1. 安装 `JDK`
    ```shell
    $ yum install java-1.8.0-openjdk java-1.8.0-openjdk-devel java-1.8.0-openjdk-javadoc
    $ vi  /etc/profile
    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.171-8.b10.el7_5.x86_64
    export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    export PATH=$PATH:$JAVA_HOME/bin
    $ source /etc/profile     #使配置文件立即生效
    ```
2. 安装 `zookeeper`
    ```shell
    $ cd /www/software
    $ wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz
    $ tar -zxvf zookeeper-3.4.9.tar.gz
    $ mv /www/software/zookeeper-3.4.9 /www/server/zookeeper
    $ vi /etc/profile
    export ZOOKEEPER_HOME=/www/server/zookeeper
    export PATH=$ZOOKEEPER_HOME/bin:$PATH
    export PATH
    $ source /etc/profile
    $ cd /www/server/zookeeper/conf
    $ cp zoo_sample.cfg zoo.cfg
    $ vi /www/server/zookeeper/conf/zoo.cfg
    # zookeeper 定义的基准时间间隔，单位：毫秒
    tickTime=2000
    # 数据文件夹
    dataDir=/www/server/zookeeper/data
    # 日志文件夹
    dataLogDir=/www/server/zookeeper/logs
    # 客户端访问 zookeeper 的端口号
    clientPort=2181
    $ /www/server/zookeeper/bin/zkServer.sh start 
    ZooKeeper JMX enabled by default
    Using config: /www/server/zookeeper/bin/../conf/zoo.cfg
    Starting zookeeper ... STARTED
    
    # 安装 `zookeeper c client`
    $ cd /www/server/zookeeper/src/c
    $ ./configure
    $ make && make install
    ```
3. 安装 `kafka`
    ```shell
    $ cd /www/software
    $ wget https://archive.apache.org/dist/kafka/1.1.0/kafka_2.12-1.1.0.tgz
    $ tar -xzvf kafka_2.12-1.1.0.tgz
    $ mv /www/software/kafka_2.12-1.1.0 /www/server/kafka
    ```
    * 配置 `kafka`，修改 `server.properties`
        ```shell
        $ mkdir /www/wwwlogs/kafka                #创建kafka日志目录
        $ cd /www/server/kafka/config             #进入配置目录
        $ vi server.properties                    #编辑修改相应的参数
        broker.id=0
        port=9092                                 #端口号
        host.name=192.168.137.254                 #服务器IP地址，修改为自己的服务器IP
        log.dirs=/www/wwwlogs/kafka               #日志存放路径，上面创建的目录
        zookeeper.connect=localhost:2181    #zookeeper地址和端口，单机配置部署，localhost:2181
        ```
    
    * 配置 `kafka` 下的 `zookeeper` 
        ```shell
        $ mkdir /www/server/zookeeper        #创建zookeeper目录
        $ mkdir /www/wwwlogs/zookeeper    #创建zookeeper日志目录
        $ cd /www/server/kafka/config              #进入配置目录
        $ vi zookeeper.properties                 #编辑修改相应的参数 
        dataDir=/www/server/kafka/zookeeper        #zookeeper数据目录 
        dataLogDir=/www/server/kafka/log/zookeeper #zookeeper日志目录 
        clientPort=2181 
        maxClientCnxns=100 
        tickTime=2000 
        initLimit=10
        ```
    
    * 创建启动 `kafka` 脚本
        ```shell
        $ vi kafkastart.sh
        #!/bin/bash
        #启动zookeeper
        /www/server/kafka/bin/zookeeper-server-start.sh /www/server/kafka/config/zookeeper.properties &
        sleep 3 #等3秒后执行
        #启动kafka
        /www/server/kafka/bin/kafka-server-start.sh /www/server/kafka/config/server.properties &
        ```
    
    * 创建关闭 `kafka` 脚本
        ```shell
        $ vi kafkastop.sh
        #!/bin/bash
        #关闭zookeeper
        /www/server/kafka/bin/zookeeper-server-stop.sh /www/server/kafka/config/zookeeper.properties &
        sleep 3 #等3秒后执行
        $ /www/server/kafka/bin/kafka-server-stop.sh /www/server/kafka/config/server.properties &
        ```
    
    * 添加脚本执行权限
        ```shell
        $ chmod +x kafkastart.sh
        $ chmod +x kafkastop.sh
        ```
    
    * 设置脚本开机自动执行
        ```shell
        $ vi /etc/rc.d/rc.local                 # 编辑，在最后添加一行
        $ sh /www/server/kafka/kafkastart.sh &   # 设置开机自动在后台运行脚本
        $ sh /www/server/kafka/kafkastart.sh     # 启动kafka
        $ sh /www/server/kafka/kafkastop.sh      # 关闭kafka
        ```
4. 测试 `kafka`
    * 步骤1：启动 `zookeeper`
      ```shell
      $ /www/server/zookeeper/bin/zkServer.sh start
      JMX enabled by default
      Using config: /home/laoyang/zookeeper/bin/../conf/zoo.cfg
      grep: /home/laoyang/zookeeper/bin/../conf/zoo.cfg: No such file or directory
      Starting zookeeper ... STARTED
      ```
    * 步骤2：启动 `kafka`
      ```shell
      $ ./kafkastart.sh
      ```
    * 步骤3：测试创建 `topic`
      ```shell
      $ cd /www/server/kafka/bin 
      $ ./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
      ```
    * 步骤4：通过 `list`命令查看创建的 `topic`
      ```shell
      $ cd /www/server/kafka/bin
      $ ./kafka-topics.sh –list –zookeeper localhost:2181
      ```
    * 步骤5：生产消息测试
      ```shell
      $ ./kafka-console-producer.sh --broker-list localhost:9092 --topic test 
      laoyang I love you! 
      ```
    * 步骤6：消费消息测试
      ```shell
      $ ./kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
      I'm laoyang #之前测试输入的内容
      laoyang I love you!
      ```
    > 经过以上6步，代表kafka安装成功。
    
    * 停止 `kafka`
      * 步骤1：停止 `Kafka`
        ```shell
        $ cd /usr/local/kafka
        $ ./kafkastop.sh
        ```
      * 步骤2：停止 `Zookeeper server`
        ```shell
        $ /www/server/zookeeper/bin/zkServer.sh stop
        ```
5. 安装 `RabbitMQ`
    1. 安装 `erlang` `RabbbitMQ` 基与 `erlang` 开发，首先安装 `erlang`，这里采用 `yum` 方式。
        1. 更新 `EPEL` 源 
            * yum官方源无 `erlang`；
            * 安装EPEL：[http://fedoraproject.org/wiki/EPEL/FAQ#howtouse]
            ```shell
            $ rpm -Uvh http://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
            $ yum install foo
            ```
        2. 添加 `erlang` 解决方案库
            * 如果不添加 `erlang` 解决方案，`yum` 安装的 `erlang` 版本会比较老；
            * 解决方案添加及安装请见（根据OS选择）：[https://www.erlang-solutions.com/resources/download.html]
              ```shell
              $ wget https://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
              $ rpm -Uvh erlang-solutions-1.0-1.noarch.rpm
              ```
            * 需要安装验证签名的公钥；
               ```shell
                $ rpm --import https://packages.erlang-solutions.com/rpm/erlang_solutions.asc
                ```
        3. 安装 `erlang`
            ```shell
            $ yum install erlang -y
            ```
    2. 安装 `RabbitMQ`
        1. 下载 `RabbitMQ`
            ```shell
            $ wget https://bintray.com/rabbitmq/rabbitmq-server-rpm/download_file?file_path=rabbitmq-server-3.6.10-1.el7.noarch.rpm
            $ mv download_file\?file_path\=rabbitmq-server-3.6.10-1.el7.noarch.rpm rabbitmq-server-3.6.10-1.el7.noarch.rpm
            ```
        2. 导入认证签名
            ```shell
            $ rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
            ```
        3. 安装
            * `yum` 安装使用已下载的 `rpm` 包本地安装，但可能会涉及到取依赖包，同样需要导入认证签名；
            * 下载安装请见：[http://www.rabbitmq.com/install-rpm.html]
            ```shell
            $ yum install rabbitmq-server-3.6.10-1.el7.noarch.rpm -y
            ```
    3. 启动
        1. 设置开机启动
            ```shell
            $ systemctl enable rabbitmq-server
            ```
        2. 启动
            ```shell
            $ systemctl start rabbitmq-server
            ```
    4. 验证
        1. 查看状态
            ```shell
            $ systemctl status rabbitmq-server
            ```
        2. 查看日志
            * 日志中给出了 `rabbitmq` 启动的重要信息，如 `node` 名，`$home` 目录，`cookie`  `hash` 值，日志文件，数据存储目录等；
            * 给出的信息会指出无配置文件（如下图），默认安装没有此文件
            ```shell
            $ cat /var/log/rabbitmq/rabbit@rmq-node1.log
            ```
    5. `rabbitmq.conf`
        * 手工建目录，将配置样例文件拷贝到配置目录并改名
          ```shell
          $ mkdir -p /etc/rabbitmq
          $ cp /usr/share/doc/rabbitmq-server-3.6.10/rabbitmq.config.example /etc/rabbitmq/rabbitmq.config
          ```
        * 配置重启生效
          ```shell
          $ systemctl restart rabbitmq-server
          ```
        > 另外还可以建环境配置文件：`/etc/rabbitmq/rabbitmq-env.conf`
    
    6. 安装web管理插件
        * `management plugin` 默认就在 `RabbitMQ` 的发布版本中，`enable` 即可
        * 服务重启，配置生效
        ```shell
        $ rabbitmq-plugins enable rabbitmq_management
        ```
    7. 设置`iptables`
        * `tcp4369` 端口用于集群邻居发现；
        * `tcp5671`，`5672` 端口用于 `AMQP 0.9.1 and 1.0 clients` 使用；
        * `tcp15672` 端口用于 `http api` 与 `rabbitadmin` 访问，后者仅限在 `management plugin` 开启时；
        * `tcp25672` 端口用于 `erlang` 分布式节点/工具通信
        ```shell
        $ vi /etc/sysconfig/iptables
        -A INPUT -p tcp -m state --state NEW -m tcp --dport 4369 -j ACCEPT
        -A INPUT -p tcp -m state --state NEW -m tcp --dport 5671 -j ACCEPT
        -A INPUT -p tcp -m state --state NEW -m tcp --dport 5672 -j ACCEPT
        -A INPUT -p tcp -m state --state NEW -m tcp --dport 15672 -j ACCEPT
        -A INPUT -p tcp -m state --state NEW -m tcp --dport 25672 -j ACCEPT
        $ service iptables restart
        ```
    
    8. `Management plugin`登录账
        1. guest账号
            * `rabbit` 默认只有 `guest` 账号，但为了安全，`guest` 账号只能从 `localhost` 登录，如果需要 `guest` 账号可以远程登录，可以设置 `rabbitmq.conf` 文件：
            * 根据说明，取消第64行参数的注释与句末的符号；建议不开启 `guest` 账号的远程登录；
            * 服务重启，配置生效。
            ```shell
            $ vi /etc/rabbitmq/rabbitmq.config
            ```
        2. CLI创建登录账号
            * `rabbitmqctl add_user` 添加账号，并设置密码
              ```shell
              $ rabbitmqctl add_user admin admin@123
              ```
            * `rabbitmqctl set_user_tags` 设置账号的状态
              ```shell
              $ rabbitmqctl set_user_tags admin administrator
              ```
            * `rabbitmqctl set_permissions` 设置账号的权限
              ```shell
              $ rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"
              ```
            * `rabbitmqctl list_users` 列出账号
              ```shell
              $ rabbitmqctl list_users
              ```
    9. `Management plugin` 登录验证
        > 浏览器访问：`http://127.0.0.1:15672/`
        
### 11. Composer安装和使用
  1. 安装 `Composer`
      ```shell
      # 下载composer.phar 
      $ wget https://dl.laravel-china.org/composer.phar -O /usr/local/bin/composer
      
      # 把composer.phar移动到环境下让其变成可执行 
      $ chmod a+x /usr/local/bin/composer
      $ composer config -g repo.packagist composer https://packagist.phpcomposer.com
      
      # 测试
      $ composer -V 
      ```
  2. 使用 `Composer` 安装 `ThinkPHP 5.1`
      ```shell
      $ composer create-project topthink/think thinkphp5.1 --prefer-dist
      $ cd thinkphp5.1
      $ composer require topthink/think-swoole
      $ composer require flc/alidayu
      $ composer require topthink/think-helper
      $ composer require topthink/think-image
      $ composer require topthink/think-captcha
      $ composer require nmred/kafka-php
      $ composer require adodb/adodb-php
      $ composer require phpoffice/phpspreadsheet
      $ composer require phpoffice/phpexcel
      $ composer require phpoffice/phpword
      $ composer require tecnickcom/tcpdf
      $ composer require mpdf/mpdf
      $ composer require gmars/tp5-qiniu
      $ composer require apache/log4php
      $ composer require ccampbell/chromephp
      ```