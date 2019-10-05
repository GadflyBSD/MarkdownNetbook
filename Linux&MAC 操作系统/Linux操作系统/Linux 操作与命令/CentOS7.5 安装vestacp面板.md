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
### 3、安装一些必备的包
```shell
$ yum -y install gcc gcc-c++ make screen wget net-tools curl python
$ screen -S bt
```

### 4、修改`CentOS`默认`yum`源为国内`yum`镜像源
1. 备份`/etc/yum.repos.d/CentOS-Base.repo`
    ```shell
    $ mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
    ```
2. 下载`163`的`yum`源配置文件到上面那个文件夹内
    ```shell
    $ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
    ```
3. 运行`yum makecache`生成缓存
    ```shell
    $ yum makecache
    ```
4. 更新系统
    ```shell
    $ yum -y update
    ```
    
### 5. 将已经挂载在 `home` 目录上的硬盘挂载到 `www` 目录上
```shell
$ df -h                   #（查看分区情况及数据盘名称）
$ mkdir /www              #（如果没有data目录就创建，否则此步跳过）
$ umount /home            #（卸载硬盘已挂载的home目录）
$ mount /dev/mapper/centos-home /www    #（挂载到data目录）
$ vi /etc/fstab           #（编辑fstab文件修改或添加，使重启后可以自动挂载）
$ mount /dev/mapper/centos-home /www ext3 auto 0 0
```

### 6、安装Vesta面板
```shell
$ curl -L https://github.com/duy13/VDVESTA/raw/master/vdvesta.sh -o vdvesta.sh
$ chmod +x vdvesta.sh
$ ./vdvesta.sh
```

### 7、安装 `PostgreSQL 10`
1. 卸载原有postgreSQL
    ```shell
    $ rpm -qa|grep postgresql
    postgresql-server-9.2.23-3.el7_4.x86_64
    postgresql-libs-9.2.23-3.el7_4.x86_64
    postgresql-contrib-9.2.23-3.el7_4.x86_64
    postgresql-9.2.23-3.el7_4.x86_64
    $ yum -y remove postgresql
    $ yum -y remove php-pgsql
    ```
2. 根据你的服务器架构添加 `PostgreSQL` 库并来安装 `PostgreSQL`
    ```shell
    $ rpm -Uvh https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm
    $ yum update
    $ yum install postgresql10-server postgresql10-contrib postgresql10-devel
    ```
3. 使用以下命令来初始化 `PostgreSQL` 数据库
    ```shell
    $ mkdir /home/pgsql
    $ chown -R postgres:postgres /home/pgsql
    $ su postgres
    $ /usr/pgsql-10/bin/initdb -D /home/pgsql/data
    $ vi /usr/lib/systemd/system/postgresql-10.service
    Environment=PGDATA=/home/pgsql/data
    $ systemctl daemon-reload
    $ systemctl start postgresql-10
    ```
4. 配置默认 `postgres` 用户及密码
    ```shell
    $ su postgres
    $ psql
    psql (10.4)
    Type "help" for help.
    Postgres=# 
    postgres=# \password postgres
    Enter new password:
    Enter it again:
    postgres=# CREATE EXTENSION adminpack;
    CREATE EXTENSION 
    postgres=# \q
    $ exit
    ```
5. 创建新的用户和数据库
    ```shell
    $ cd home
    $ su postgres
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
6. 配置 `PostgreSQL-MD5` 认证
    ```shell
    $ vi /home/pgsql/data/pg_hba.conf
    # "local" is for Unix domain socket connections only
    local        all      all                     md5
    # IPv4 local connections:
    host        all      all    192.168.1.0/24    md5
    host        all      all    127.0.0.1/32      md5
    # IPv6 local connections:
    host        all      all    ::1/128           md5
    ```
7. 配置 `PostgreSQL-Configure TCP/IP`
    ```shell
    $ vi /home/pgsql/data/postgresql.conf 
    [...]
    listen_addresses = '*’
    [...]
    port = 5432
    [...]
    ```
8. 启动 `PostgreSQL` 服务并使之开机自启
    ```shell
    $ systemctl enable postgresql-10
    $ systemctl restart postgresql-10
    ```
9. 安装 `redis_fdw` 外部表插件
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