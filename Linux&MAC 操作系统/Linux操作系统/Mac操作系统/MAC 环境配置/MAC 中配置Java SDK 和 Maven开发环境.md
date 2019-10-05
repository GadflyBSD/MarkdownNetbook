## 一、卸载`Java SDK`的方法
```shell
sudo rm -rf /Library/Java/JavaVirtualMachines/jdk-9.jdk
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin 
sudo rm -fr /Library/PreferencePanes/JavaControlPanel.prefPane 
sudo rm -fr ~/Library/Application\ Support/Java
```

## 二、配置新的`Java`开发环境
### 1. 下载JDK
从下面链接选择合适版本的安装包进行下载...笔者下载的是jdk-9.0.1
链接：http://www.oracle.com/technetwork/java/javase/downloads/index.html
### 2. 安装JDK
双击jdk-9.0.1_osx-x64_bin.dmg文件进行安装
### 3. 查看是否安装成功
打开terminal，输入：java -version
```shell
$ java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```
### 4. 配置PATH和CALSSPATH路径
打开terminal，打开profile文件（需要输入密码）
```shell
$ sudo vi /etc/profile
Password:
# 在文件末尾添加JAVA_HOME路径（切换英文输入法，键入“i”，进入插入模式）
JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home/"
CLASS_PATH="$JAVA_HOME/lib"
PATH=".:$PATH:$JAVA_HOME/bin"
```
### 5. 输入以下命令使生效
```shell
$ source /etc/profile
```
### 6. 查看更改后的JAVA_HOME路径
```shell
$ echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk-9.0.1.jdk/Contents/Home/
```

### 三、安装、配置Maven
#### 1. 下载maven， http://maven.apache.org/download.cgi 下载，选择binary zip archive 的类型。
#### 2. 解压maven 解压下载好的maven’，将目录丢到终端命令去获取文件路径。设置path： 
```shell
sudo vi /etc/profile 
# 添加maven的路径，将下载好的maven资源引入path 中：
M2_HOME=/Library/Java/JavaVirtualMachines/apache-maven-3.5.4
PATH=".:$PATH:$JAVA_HOME/bin:$M2_HOME/bin"
```
#### 3. 使path设置生效 设置path后，使用以下命令使之生效：
```shell
source /etc/profile
```
#### 4. 查看maven配置是否生效 
使用 man -v 命令查看mvn命令是否ok
```shell
$ mvn -v
Apache Maven 3.5.4 (1edded0938998edf8bf061f1ceb3cfdeccf443fe; 2018-06-18T02:33:14+08:00)
Maven home: /Library/Java/JavaVirtualMachines/apache-maven-3.5.4
Java version: 1.8.0_181, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.13.6", arch: "x86_64", family: "mac"
```