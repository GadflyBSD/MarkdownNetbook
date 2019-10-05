## 查看当前防火墙规则
```shell
iptables -L -n
```

## 添加一个开放端口22的输入流的规则
```shell
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

## 添加一个开放端口22的输出流的规则
```shell
iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT
```
## 保存上述规则
```shell
service iptables save
```

## 查看iptables服务状态
```shell
service iptables status
```

## 打开iptables的配置文件
```shell
vi /etc/sysconfig/iptables
```

## 最后重启防火墙使配置生效
```shell
systemctl restart iptables.service
```

## 设置防火墙开机启动
```shell
systemctl enable iptables.service
```

## 禁止iptables服务
```shell
systemctl disable iptables
```

## 暂停服务
```shell
systemctl stop iptables
```

## 解除禁止iptables
```shell
systemctl enable iptables
```

## 开启服务
```shell
systemctl start iptables 
```

## 清空已有的规则
```shell
iptables -F
iptables -X
iptables -Z
```

# iptables的基本参数
参数|作用
--|--
-P|设置默认策略:iptables -P INPUT (DROP|ACCEPT)
-F|清空规则链
-L|查看规则链
-t|表名（默认为filter）
-A|在规则链的末尾加入新规则
-I num|在规则链的头部加入新规则
-D num|删除某一条规则
-s ip|匹配来源地址IP/MASK，加叹号"!"表示除这个IP外
-d ip|匹配目标地址IP/MASK，加叹号"!"表示除这个IP外
-i 网卡名称|匹配从这块网卡流入的数据
-o 网卡名称|匹配从这块网卡流出的数据
-p|匹配协议,如tcp,udp,icmp
--dport num|匹配目标端口号
--sport num|匹配来源端口号

# iptables命令中则常见的控制类型
动作参数|参数说明
--|--
ACCEPT|对满足策略的数据包允许通过
DROP|丢弃数据包，且不返回任何信息
REJECT|丢弃数据包，但是会返回拒绝的信息
LOG|把通过的数据包写到日志中

# 规则链则依据处理数据包的位置不同而进行分类
数据包链参数|参数说明
--|--
INPUT|处理入站的数据包
OUTPUT|处理出站的数据包
FORWARD|处理转发的数据包
PREROUTING|在进行路由选择前处理数据包
POSTROUTING|在进行路由选择后处理数据包

> 1. 如果数据包的目的地址是本机，则系统将数据包送往Input链。如果通过规则检查，则该包被发给相应的本地进程处理；如果没有通过规则检查，系统就会将这个包丢掉。
> 2. 如果数据包的目的地址不是本机，也就是说，这个包将被转发，则系统将数据包送往Forward链。如果通过规则检查，则该包被发给相应的本地进程处理;如果没有通过规则检查，系统就会将这个包丢掉。
> 3. 如果数据包是由本地系统进程产生的，则系统将其送往Output链。如果通过规则检查，则该包被发给相应的本地进程处理；如果没有通过规则检查，系统就会将这个包丢掉。

> input只对要访问本机的包有效，forward只对不是访问本机，但是要通过这台机器转发的包有效

> 制定规则的时候有个原则就是 放行的要先放在前面, 禁止的要放在最后.