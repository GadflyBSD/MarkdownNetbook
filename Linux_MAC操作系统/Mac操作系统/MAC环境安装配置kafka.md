### 一、安装kafka
```

```

### 二、一键启动zookeeper和kafka
```
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties & kafka-server-start /usr/local/etc/kafka/server.properties &
```
或者
```
brew services start kafka
```

### 三、创建topic
> 让我们使用单个分区和只有一个副本创建一个名为“test”的主题

```
/bin/kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

### 四、查看创建的topic
```
kafka-topics --list --zookeeper localhost:2181
```

### 五、发送一些消息
> Kafka提供了一个命令行客户端，它将从文件或标准输入接收输入，并将其作为消息发送到Kafka集群。默认情况下，每行都将作为单独的消息发送。运行生产者，然后在控制台中键入一些消息发送到服务器。

```
kafka-console-producer --broker-list localhost:9092 --topic test 
```

### 六、消费消息
```
kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
```

### 七、下载gradle 
```
brew update && brew install gradle
```

### 八、在IDEA中选择Use default gradle wrapper ，不要选择Use local gradle distribution 
> 此外配置gradle home：/usr/local/opt/gradle/libexec和/usr/local/Cellar/gradle/X.X/libexec 都可以（X.X是版本号）

如果发生报错Gradle-sync failed: No such property: useAnt for class: org.gradle.api.tasks.scala.ScalaCompileOptions 
则在build.gradle文件中加入几行即可：
```
ScalaCompileOptions.metaClass.daemonServer = true
ScalaCompileOptions.metaClass.fork = true
ScalaCompileOptions.metaClass.useAnt = false
ScalaCompileOptions.metaClass.useCompileDaemon = false
```

### 九、构建产生IDEA工程文件
> 在项目根目录下执行命令：`gradle idea`

### 十、路径
```
cd /usr/local/etc/kafka
cd /usr/local/Cellar/kafka/2.0.0/libexec/bin/
```