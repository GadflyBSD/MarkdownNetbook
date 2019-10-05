### 1. 禁用系统完整性保护System Integrity Protection (SIP) 
  1. 重启系统
  2. 按住Command + R   （重新亮屏之后就开始按，象征地按几秒再松开，出现苹果标志，ok）
  3. 菜单“实用工具” ==>> “终端” ==>> 输入 `csrutil disable`；
  4. 执行后会输出：
  `Successfully disabled System Integrity Protection. Please restart the machine for the changes to take effect.`
  5. 再次重启系统
  6. 禁止掉SIP后，就可以顺利的安装了，当然装完了以后你可以重新打开SIP，方法同上，只是命令是 `csrutil enable`

### 2. 在【安全性与隐私】窗口中没有【任何来源】的解决方法
```shell
$ sudo spctl --master-disable
```
  
### 2. Homebrew安装 和 通过 brew 安装 autoconf
```shell
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install autoconf
brew install libmemcached
```

### 3. 安装Xcode环境和命令行开发工具
```shell
xcode-select --install
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```

### 4. 安装PEAR 和 PECL
```shell
cd /usr/lib/php
sudo curl -O http://pear.php.net/go-pear.phar
sudo php -d detect_unicode=0 go-pear.phar
sudo pear upgrade-all
sudo pecl channel-update pecl.php.net
```
> 执行以上命令后会进行安装过程，会有一些配置选项
> * 输入1，回车，配置pear路径为：`/usr/local/pear`
> * 输入4，回车，配置命令路径为：`/usr/local/bin`
> * 回车两次，其他让其默认，安装完成
> * 可以通过命令检查pear安装是否成功 `pear version`

### 5. 安装 PostgreSQL
```shell
$ brew install postgresql
$ brew services start postgresql
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
$ cp /usr/local/Cellar/postgresql/10.5/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/
$ launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

### 5. 安装pkg-config
```shell
$ brew install pkg-config
$ brew install ImageMagick
$ brew install pcre
$ brew install libevent
$ brew install libmemcached
$ brew install openssl
LD_LIBRARY_PATH=/usr/local/Cellar/openssl/1.0.2p/lib:"${LD_LIBRARY_PATH}"
CPATH=/usr/local/Cellar/openssl/1.0.2p/include:"${CPATH}"
PKG_CONFIG_PATH=/usr/local/Cellar/openssl/1.0.2p/lib/pkgconfig:"${PKG_CONFIG_PATH}"
export LD_LIBRARY_PATH CPATH PKG_CONFIG_PATH
source ~/.bashrc
$ brew install mcrypt 
$ brew install gearmand
$ brew install hiredis
$ brew install libpq
echo 'export PATH="/usr/local/opt/libpq/bin:$PATH"' >> ~/.zshrc
export LDFLAGS="-L/usr/local/opt/libpq/lib"
export CPPFLAGS="-I/usr/local/opt/libpq/include"
export PKG_CONFIG_PATH="/usr/local/opt/libpq/lib/pkgconfig"
source ~/.zshrc
$ brew install librdkafka
$ brew install rabbitmq-c
$ brew install zookeeper
```

### 5. 安装PHP扩展库
  * 安装 Memcached
    ```shell
    sudo pecl install memcached
    # 输入libmemcached路径：
    libmemcached directory [no] : /usr/local/Cellar/libmemcached/1.0.18_2/
    ```
  * 安装其他扩展
    ```shell
    sudo pecl install mcrypt-1.0.0 mongodb igbinary redis xdebug swoole imagick gearman rdkafka amqp zookeeper
    # 输入librabbitmq路径
    Set the path to librabbitmq install prefix [autodetect] : /usr/local/Cellar/rabbitmq-c/0.9.0
    ```
  * 修改php.ini配置文件
    ```shell
    $ sudo cp /etc/php.ini.default /etc/php.ini
    $ sudo chmod 0644 /etc/php.ini
    $ sudo vi /etc/php.ini
    ```
    > 在`/etc/php.ini`中添加
    ```ini
    extension=memcached.so
    extension=mcrypt.so
    extension=mongodb.so
    extension=redis.so
    extension=igbinary.so
    extension=imagick.so
    extension=swoole.so
    extension=mongodb.so
    extension=rdkafka.so
    extension=amqp.so
    extension=zookeeper.so
    zend_extension=xdebug.so
    zend_extension=opcache.so
    ［opcache］
    ; Determines if Zend OPCache is enabled
    opcache.enable=0
    ; Determines if Zend OPCache is enabled for the CLI version of PHP
    opcache.enable_cli=0
    ```
  * 修改`httpd.conf`文件
    > 打开编辑`/etc/apache2/httpd.conf`文件
    ```shell
    $ sudo vi /etc/apache2/httpd.conf
    ```
    > 去掉下面代码最前面的`#`
    ```ini
    #LoadModule authn_core_module libexec/apache2/mod_authn_core.so
    #LoadModule authz_host_module libexec/apache2/mod_authz_host.so
    #LoadModule dir_module libexec/apache2/mod_dir.so
    #LoadModule userdir_module libexec/apache2/mod_userdir.so
    #LoadModule alias_module libexec/apache2/mod_alias.so
    #LoadModule rewrite_module libexec/apache2/mod_rewrite.so
    #LoadModule php5_module libexec/apache2/libphp7.so
    ```
    > 找到`/Library/WebServer/Documents`, 把将其修改为自己的网站根目录
    ```ini
    DocumentRoot "/Users/gadflybsd/jetbrainsPhpProjects/docs.cn"
    <Directory "/Users/gadflybsd/jetbrainsPhpProjects/docs.cn">
    ```
    > 找到`DirectoryIndex index.html` 修改为：
    ```ini
    DirectoryIndex index.html default.html index.php default.php
    ```
    > 找到下列代码，去掉下面代码最前面的`#`
    ```ini
    #Include /private/etc/apache2/extra/httpd-userdir.conf
    #Include /private/etc/apache2/extra/httpd-vhosts.conf
    #Include /private/etc/apache2/other/*.conf
    ```
  * 创建虚拟主机
    1. 添加`/etc/apache2/extra/httpd-vhosts.conf`虚拟主机配置
        ```shell
        $ sudo vi /etc/apache2/extra/httpd-vhosts.conf
        <VirtualHost *:80>
          ServerAdmin webmaster@dummy-host.example.com
          DocumentRoot "/Users/gadflybsd/jetbrainsPhpProjects/docs.cn"
          ServerName localhost
          ServerAlias localhost
          ErrorLog "/private/var/log/apache2/localhost-error_log"
          CustomLog "/private/var/log/apache2/localhost-access_log" common
        </VirtualHost>
        
        <VirtualHost *:80>
          ServerAdmin webmaster@dummy-host2.example.com
          DocumentRoot "/Users/gadflybsd/jetbrainsPhpProjects/docs.cn"
          ServerName docs.cn
          ErrorLog "/private/var/log/apache2/docs.cn-error_log"
          CustomLog "/private/var/log/apache2/docs.cn-access_log" common
        </VirtualHost>
        
        <VirtualHost *:80>
          DocumentRoot "/Users/gadflybsd/jetbrainsPhpProjects/masses_report/client/app"
          ServerName app.masses.cn
          ErrorLog "/private/var/log/apache2/app.masses-error_log"
          CustomLog "/private/var/log/apache2/app.masses-access_log" common
          <Directory />
            Options Indexes FollowSymLinks MultiViews
            AllowOverride None
            Order deny,allow
            Allow from all
          </Directory>
        </VirtualHost>
        ```
    2. 修改虚拟主机根目录文件夹权限
        ```shell
        sudo chmod -R 775 /Users/gadflybsd/jetbrainsPhpProjects
        ```
    3. 添加dns解析
        ```shell
        $ sudo vi /etc/hosts
        127.0.0.1   docs.cn
        127.0.0.1   app.masses.cn
        ```
  * 重启`Apache`服务器
    ```shell
    sudo apachectl restart
    ```
### 6. 配置mysql并安装
```shell
git clone https://github.com/mysqludf/lib_mysqludf_sys.git
cd lib_mysqludf_sys
sudo cp lib_mysqludf_sys.so /usr/local/mysql/lib/plugin/
sudo chmod 777 /usr/local/mysql/lib/plugin/lib_mysqludf_sys.so
/usr/local/mysql/bin/mysql -uroot -p
```

### 7. MySQL 修改`root`密码
#### 方法1： 用SET PASSWORD命令
```shell
MySQL -u root
mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpass');
```

#### 方法2：用mysqladmin
```shell
mysqladmin -u root password "newpass"
```
> 如果root已经设置过密码，采用如下方法
```shell
mysqladmin -u root password oldpass "newpass"
```

#### 方法3： 用UPDATE直接编辑user表
```shell
mysql -u root
mysql> use mysql;
mysql> UPDATE user SET Password = PASSWORD('newpass') WHERE user = 'root';
mysql> FLUSH PRIVILEGES;
```

> 在丢失root密码的时候，可以这样
```shell
mysqld_safe --skip-grant-tables&mysql -u root mysql
mysql> UPDATE user SET password=PASSWORD("new password") WHERE user='root';
mysql> FLUSH PRIVILEGES;
```

### 8. 配置SSH免密码登录
> sshpass: 用于非交互的ssh 密码验证ssh登陆不能在命令行中指定密码
```shell
$ brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb
```
> 使用方法
```shell
sshpass -p 'ssh_password' ssh ssh_user@xxx.xxx.xxx.xxx 
```