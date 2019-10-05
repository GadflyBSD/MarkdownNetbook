# 一、安装Redis
  * 下载redis
    ```shell
    wget   http://download.redis.io/redis-stable.tar.gz
    tar -zxvf redis-stable.tar.gz
    cd redis-stable
    make
    make install
    ```
  * 在redis安装目录下进入utils目录，执行自动安装脚本
    ```shell
    $ cd utils/
    $ ./install_server.sh     // 一路回车都按照默认设置执行
    //执行完脚本后，会出现以下提示：
    Selected config:
    Port           : 6379
    Config file    : /etc/redis/6379.conf
    Log file       : /var/log/redis_6379.log
    Data dir       : /var/lib/redis/6379
    Executable     : /usr/local/bin/redis-server
    Cli Executable : /usr/local/bin/redis-cli
    ```
    
  * 添加redis开机自启动
    ```shell
    //修改文件权限
    chmod 755 /etc/init.d/redis_6379
    //添加自启动
    chkconfig --add redis_6379
    chkconfig --level 345 redis_6379 on
    ```
  * 检查远程服务器的6379端口是否被防火墙拦截。假如未开启，则开添加
    ```shell
    /sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT
    /etc/init.d/iptables restart(视服务器情况而定，如果不知道的话可以选择重启服务器)
    也可以在wdcp的后台系统管理--iptables添加规则
    ```
    
  * 通过客户端命令行连接redis 
    ```shell
    redis-cli -h 127.0.0.1 -p 6379
    ```

# 二、编译安装hiredis
  * hiredis下载地址 [releases](https://github.com/redis/hiredis/releases)
  * 编译安装
    ```shell
    make -j
    sudo make install
    sudo ldconfig // (编译安装完记得执行该命令，否则PHP在引入swoole扩展时将出现错误)
    ```

# 三、为PHP添加phpredis扩展
  * 获取并解压安装包
    ```shell
    cd ~
    wget  https://github.com/phpredis/phpredis/archive/develop.zip
    unzip develop.zip
    cd phpredis-develop
    ```
  * 使用phpize命令添加扩展，phpize命令所在路径根据实际情况修改
    ```shell
    /www/wdlinux/php/bin/phpize
    ./configure --with-php-config=/www/wdlinux/php/bin/php-config
    make
    make install
    ```
  * php.ini中加入redis.so扩展
    ```shell
    $ vi /www/wdlinux/php/etc/php.ini
    //加入这一行，保存退出。路径要使用上面装完redis生成redis.so的路径
    extension=/www/wdlinux/nginx_php /lib/php/extensions/no-debug-non-zts-20121212/redis.so
    ```
  * 通过phpinfo查看是否添加了redis扩展
    ```shell
    /www/wdlinux/init.d/httpd restart
    /www/wdlinux/php/bin/php -m | grep redis
    ```
    
# 四、启动/关闭Redis服务命令
  ```shell
  //查看是否启动redis服务
  ps -ef | grep redis
  //启动
  etc/init.d/redis_6379  start
  //通过配置文件启动
  usr/local/bin/redis-server /etc/redis/6379.conf
  //关闭
  etc/init.d/redis_6379 stop
  //关闭，假如是默认端口号6379，可以省略 -p 6379参数
  usr/local/bin/redis-cli -p 6379 shutdown
  ```
  
# 五、为PHP添加swoole扩展
  * swoole 下载地址 [releases](https://github.com/swoole/swoole-src/releases)
    * Swoole-1.x需要 PHP-5.3.10 或更高版本
    * Swoole-2.x需要 PHP-7.0.0 或更高版本
  * 进入源码目录，执行下面的命令进行编译和安装
    ```shell
    $ cd swoole
    $ /www/wdlinux/php/bin/phpize
    $ ./configure --enable-async-redis --with-php-config=/www/wdlinux/php/bin/php-config  --enable-openssl --enable-swoole-debug  --enable-sockets --enable-swoole
    $ make 
    $ sudo make install
    ```
  * 配置php.ini
    编译安装成功后，修改php.ini加入
    ```shell
    extension=swoole.so
    ```
  * 通过php -m或phpinfo()来查看是否成功加载了swoole
    ```shell
    /www/wdlinux/init.d/httpd restart
    /www/wdlinux/php/bin/php -m | grep swoole
    ```
  * 问题: 缺少hiredis
    ```shell
    $ vi ~/.bash_profile  \\ 在最后一行加入, 保存退出
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
    $ source ~/.bash_profile
    $ /www/wdlinux/init.d/httpd restart
    $ /www/wdlinux/php/bin/php -m | grep swoole