* # 在不知程序端口号的情况下
```shell
ps -axu 本机名|grep 程序名
```

* # 在知道程序端口的情况下
```shell
sudo lsof -i:PortNum
```

* # 结束进程
```shell
sudo kill PID号
```