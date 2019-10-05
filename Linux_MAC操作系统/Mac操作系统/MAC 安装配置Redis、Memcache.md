## 一、Redis 的安装和配置
* 安装
  ``` shell
  brew install redis
  ```
  
* 配置
  ```shell
  #修改为守护模式
  daemonize yes

  #设置进程锁文件
  pidfile /usr/local/redis/redis.pid

  #端口
  port 6379

  #客户端超时时间
  timeout 300

  #日志级别
  loglevel debug

  #日志文件位置
  logfile /usr/local/redis/log-redis.log

  #设置数据库的数量，默认数据库为16，可以使用SELECT 命令在连接上指定数据库id
  databases 16

  ##指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
  #save <seconds> <changes>
  #Redis默认配置文件中提供了三个条件：
  save 900 1
  save 300 10
  save 60 10000

  #指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，
  #可以关闭该#选项，但会导致数据库文件变的巨大
  rdbcompression yes

  #指定本地数据库文件名
  dbfilename dump.rdb

  #指定本地数据库路径
  dir /usr/local/redis/db/

  #指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中
  appendonly no

  #指定更新日志条件，共有3个可选值：
  #no：表示等操作系统进行数据缓存同步到磁盘（快）
  #always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
  #everysec：表示每秒同步一次（折衷，默认值）
  appendfsync everysec
  ```

* 后台运行
  ``` shell
  brew services start redis
  ```

* 前台运行
  ``` shell
  redis-server /usr/local/etc/redis.conf
  ```

* 开机启动 `redis`
  ``` shell
  ln -svf /usr/local/Cellar/redis/4.0.11/*.plist ~/Library/LaunchAgents
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
  ```
  
* 停止 `Redis` 开机启动
  ``` shell 
  launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
  ```

* 卸载redis和它的文件
  ```shell
  brew uninstall redis
  rm ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
  ```

* 测试redis server是否启动
  ``` ashell
  redis-cli ping
  ```
* 停止redis server服务
  ``` shell
  redis-cli shutdown
  ```

