### 1、准备
  ```shell
  # 安装之前先检查一下系统是否有默认安装的`apache`或者`php`
  $ rpm -qa|grep httpd
  $ rpm -qa|grep php
  $ rpm -qa|grep mysql
  
  # 把上面指令列出来的包删除
  $ rpm -e ****(包名)
  
  # 安装一些必备的包
  $ yum -y install gcc gcc-c++ make screen wget net-tools curl python
  $ screen -S bt
  
  # 编译安装`hiredis`
  $ git clone https://github.com/redis/hiredis.git
  $ cd hiredis
  $ make && make install
  $ mkdir /usr/lib/hiredis
  $ cp libhiredis.so /usr/lib/hiredis
  $ mkdir /usr/include/hiredis
  $ cp hiredis.h /usr/include/hiredis
  $ echo '/usr/local/lib' >>/etc/ld.so.conf 
  $ ldconfig
  ```
### 2、修改`CentOS`默认`yum`源为国内`yum`镜像源
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
    
### 3. 将已经挂载在 `home` 目录上的硬盘挂载到 `data` 目录上
```shell
$ df -h                   #（查看分区情况及数据盘名称）
$ mkdir /data              #（如果没有data目录就创建，否则此步跳过）
$ umount /home            #（卸载硬盘已挂载的home目录）
$ mount /dev/mapper/centos-home /data    #（挂载到data目录）
$ vi /etc/fstab           #（编辑fstab文件修改或添加，使重启后可以自动挂载）
$ mount /dev/mapper/centos-home /data ext3 auto 0 0
```

### 4. 交互式安装 `OneinStack`
  * 安装 `OneinStack`
    ```shell
    $ wget http://mirrors.linuxeye.com/oneinstack-full.tar.gz
    $ tar xzf oneinstack-full.tar.gz
    $ cd oneinstack
    $ screen -S oneinstack 
    # 如果网路出现中断，可以执行命令`screen -R oneinstack`重新连接安装窗口
    $ ./install.sh          # 安装
    $ ./addons.sh           # 添加附加组件
    $ ./vhost.sh            # 添加虚拟主机
    $ ./pureftpd_vhost.sh   # 管理FTP账号
    $ ./backup_setup.sh     # 备份
    $ ./upgrade.sh          # 更新版本
    $ ./uninstall.sh        # 卸载
    ```
  * 管理服务
    ```shell
    # webmin
    $ rpm -Uvh https://prdownloads.sourceforge.net/webadmin/webmin-1.881-1.noarch.rpm
    
    # Nginx/Tengine/OpenResty
    $ service nginx {start|stop|status|restart|reload|configtest}
    
    # MySQL/MariaDB/Percona:
    $ service mysqld {start|stop|restart|reload|status}
    
    # PostgreSQL
    $ service postgresql {start|stop|restart|status}
    
    # MongoDB
    $ service mongod {start|stop|status|restart|reload}
    
    # PHP
    $ service php-fpm {start|stop|restart|reload|status}
    
    # HHVM
    $ service supervisord {start|stop|status|restart|reload}
    
    # Apache
    $ service httpd {start|restart|stop}
    
    # Tomcat
    $ service tomcat {start|stop|status|restart}
    
    # Pure-Ftpd
    $ service pureftpd {start|stop|restart|status}
    
    # Redis
    $ service redis-server {start|stop|status|restart}
    
    # Memcached
    $ service memcached {start|stop|status|restart|reload}
    ```
### 5. 安装 `Gearman`
  ```shell
  $ yum install -y uuid-devel libuuid libuuid-devel uuid boost-devel libevent libevent-devel gperf
  $ wget https://launchpad.net/gearmand/1.2/1.1.12/+download/gearmand-1.1.12.tar.gz
  $ tar zxvf gearmand-1.1.12.tar.gz
  $ cd gearmand-1.1.12
  $ ./configure --prefix=/usr/local/gearmand
  $ make && make install
  $ vi /etc/init.d/gearmand
  #!/bin/bash  
  # chkconfig: - 85 15  
  #descrīption: service(/usr/local/gearmand/sbin/gearmand)  
  . /etc/rc.d/init.d/functions  
  start() {  
     echo -n $"Starting $prog"  
     echo -e " gearman ：                                               [确定]"  
     /usr/local/gearmand/sbin/gearmand &  
     sleep 1  
     echo -e "running..."  
  }  
  stop() {  
     echo -n $"Stopping $prog"  
     echo -e " gearman ：                                               [确定]"  
     kill -9 `ps -ef | grep "/usr/local/gearmand/sbin/gearmand" | awk '{print $2}' | awk 'NR==1'`  
     sleep 1  
     echo -e "stoped"  
  }  
  case "$1" in  
     start)  
        start  
        ;;  
     stop)  
        stop  
        ;;  
     restart)  
        stop  
        start  
        ;;  
     status)  
        ps -ef | grep "/usr/local/gearmand/sbin/gearmand"  
        ;;  
     *)  
        echo $"Usage: $prog {start|stop|restart|status}" >&2  
        exit 1  
        ;;  
  esac  
  exit 0  
  $ chkconfig --add gearmand
  $ chkconfig gearmand on
  $ chkconfig --list
  ```
### 6. 安装 `PostgreSQL`
  1. 安装
      ```shell
      $ yum install https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-centos96-9.6-3.noarch.rpm
      $ yum install postgresql96 postgresql96-server postgresql96-contrib postgresql96-devel postgresql96-libs
      
      $ yum install https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7.7-x86_64/pgdg-centos11-11-2.noarch.rpm
      $ yum install postgresql11.x86_64 postgresql11-devel.x86_64 postgresql11-plpython.x86_64 postgresql11-server.x86_64 postgresql11-contrib.x86_64 postgresql11-libs.x86_64
      ```
  2. 初始化 `PostgreSQL` 数据库
      ```shell
      $ mkdir /data/pgsql
      $ chown -R postgres:postgres /data/pgsql
      $ su postgres
      $ /usr/pgsql-11/bin/initdb -D /data/pgsql/data
      $ vi /usr/lib/systemd/system/postgresql-11.service
      Environment=PGDATA=/data/pgsql/data
      $ systemctl daemon-reload
      $ systemctl enable postgresql-11
      $ systemctl start postgresql-11
      ```
  3. 配置默认 `postgres` 用户及密码并创建新的用户和数据库
      ```shell
      $ cd data
      $ su postgres
      $ createuser gadfly
      $ createdb gadfly_db
      $ psql
      psql (11.2)
      Type "help" for help.
      Postgres=# 
      postgres=# \password postgres
      Enter new password:
      Enter it again:
      postgres=# CREATE EXTENSION adminpack;
      CREATE EXTENSION
      postgres=# alter user gadfly with encrypted password 'centos';
      ALTER ROLE 
      postgres=# grant all privileges on database gadfly_db to gadfly;
      GRANT
      postgres=# \q
      $ exit
      ```
  4. 配置 `PostgreSQL`
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
  5. 安装 `redis_fdw` 外部表插件
      ```shell
      $ git clone -b REL_10_STABLE https://github.com/pg-redis-fdw/redis_fdw.git
      $ cd redis_fdw
      $ export PATH=/usr/pgsql-9.6//bin:$PATH
      $ make USE_PGXS=1
      $ make USE_PGXS=1 install
      $ su postgres
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
  6. 安装`www_fdw`扩展
  
      ```shell
      $ git clone https://github.com/cyga/www_fdw.git
      $ cd www_fdw
      $ export USE_PGXS=1
      $ make && make install
      ```
  7. 安装`pgsql-http`扩展
      ```
       $ git clone https://github.com/pramsey/pgsql-http.git
      $ cd pgsql-http
      $ export PATH=/usr/local/Cellar/postgresql/11.2/bin:$PATH
      $ make USE_PGXS=1
      $ make USE_PGXS=1 install
      $ su postgres
      bash-4.2$ psql -U postgres -d postgres
      postgres=# create extension http;
      ```
  8. 安装`mysql-fdw`扩展
      ```
      git clone https://github.com/EnterpriseDB/mysql_fdw.git
      cd mysql_fdw
      $ export PATH=/usr/local/Cellar/postgresql/11.2/bin:$PATH
      $ make USE_PGXS=1
      $ make USE_PGXS=1 install
      $ su postgres
      bash-4.2$ psql -U postgres -d postgres
      postgres=# create extension mysql_fdw;
      ```
      
### 7. 安装`MySQL 5.7`的`UDF`函数
  * 在 `CentOS7` 系统上安装 `mysql-udf-http`
      ```shell
      $ export PKG_CONFIG=/usr/bin/pkg-config
      $ export PKG_CONFIG_PATH=/usr/share/pkgconfig:/usr/lib/pkgconfig
      $ echo "/usr/local/mysql/lib" > /etc/ld.so.conf.d/mysql.conf
      $ /sbin/ldconfig
      $ git clone https://github.com/y-ken/mysql-udf-http.git
      $ cd mysql-udf-http
      $ chmod +x ./configure
      $ ./configure --with-mysql=/usr/local/mysql/bin/mysql_config --prefix=/usr --libdir=/usr/local/mysql/lib/plugin
      $ make && make install
      $ cd ../
      ```
  * 在 `CentOS7` 系统上安装 `mysql-udf-sys`
      ```shell
      $ git clone https://github.com/mysqludf/lib_mysqludf_sys.git
      $ cd lib_mysqludf_sys
      $ gcc -DMYSQL_DYNAMIC_PLUGIN -fPIC -Wall -I/usr/local/mysql/include -I. -shared lib_mysqludf_sys.c -o lib_mysqludf_sys.so
      $ cp lib_mysqludf_sys.so /usr/local/mysql/lib/plugin
      ```
  * 在 `CentOS7` 系统上安装 `gearman-mysql-udf`
      ```shell
      $ wget https://launchpad.net/gearman-mysql-udf/trunk/0.6/+download/gearman-mysql-udf-0.6.tar.gz
      $ tar xf gearman-mysql-udf-0.6.tar.gz -C ./  
      $ cd gearman-mysql-udf-0.6  
      $ ./configure --with-mysql=/usr/local/mysql/bin/mysql_config --libdir=/usr/local/mysql/lib/plugin 
      $ make && make install
      ```
  * 创建 `MySQL` 自定义函数
    ```shell
    mysql> DROP FUNCTION IF EXISTS http_get; 
    mysql> DROP FUNCTION IF EXISTS http_post; 
    mysql> DROP FUNCTION IF EXISTS http_put; 
    mysql> DROP FUNCTION IF EXISTS http_delete; 
    mysql> create function http_get returns string soname 'mysql-udf-http.so';
    mysql> create function http_post returns string soname 'mysql-udf-http.so'; 
    mysql> create function http_put returns string soname 'mysql-udf-http.so'; 
    mysql> create function http_delete returns string soname 'mysql-udf-http.so';
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
    mysql> DROP FUNCTION IF EXISTS gman_do_background;
    mysql> DROP FUNCTION IF EXISTS gman_servers_set;
    mysql> CREATE FUNCTION gman_do_background RETURNS STRING SONAME 'libgearman_mysql_udf.so';
    mysql> CREATE FUNCTION gman_servers_set RETURNS STRING SONAME 'libgearman_mysql_udf.so';
    ``` 
### 8. 为 `PHP` 添加扩展
  1. 添加 `PostgreSQL` 扩展
      ```shell
      $ cd ~/oneinstack/src
      $ tar zxvf php-7.2.6.tar.gz
      $ cd php-7.2.6/ext/pgsql
      $ /usr/local/php/bin/phpize
      $ ./configure --with-php-config=/usr/local/php/bin/php-config --with-pgsql=/usr/pgsql-9.6
      $ make && make install
      ```
  2. 添加 `PDO-PostgreSQL` 扩展
      ```shell
      $ cd ~/oneinstack/src/php-7.2.6/ext/pdo_pgsql
      $ /usr/local/php/bin/phpize
      $ ./configure --with-php-config=/usr/local/php/bin/php-config --with-pdo-pgsql=/usr/pgsql-9.6
      $ make && make install
      ```
  3. 添加 `Gearman` 扩展
      ```shell
      $ git clone https://github.com/wcgallego/pecl-gearman.git
      $ cd pecl-gearman
      $ /usr/local/php/bin/phpize
      $ ./configure --with-php-config=/usr/local/php/bin/php-config --with-gearman=/usr/local/gearmand/
      $ make && make install
      ```
  4. 配置 `PHP` 支持自定义扩展
      ```shell
      $ vi /usr/local/php/etc/php.d/90-pgsql.ini
      extension=pgsql.so
      extension=pdo_pgsql.so
      extension=gearman.so
      $ service httpd restart
      ```