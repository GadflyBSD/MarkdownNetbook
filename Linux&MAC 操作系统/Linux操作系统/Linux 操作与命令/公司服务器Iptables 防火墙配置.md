```shell
iptables -P INPUT ACCEPT
iptables -F 
iptables -X 
iptables -Z 
iptables -A INPUT -i lo -j REJECT -m comment --comment "拒绝所有来自互联网的访问"
iptables -A INPUT -s 192.168.199.10 -p all -j REJECT  -m comment --comment "拒绝所有来自路由器的访问"
iptables -A INPUT -m mac --mac-source B8:08:CF:DE:D3:34 -p all -j ACCEPT -m comment --comment "邓淑云，192.168.199.13"
iptables -A INPUT -m mac --mac-source f4:5c:89:9f:1c:cd -p all -j ACCEPT -m comment --comment "刘海明，192.168.199.37"
iptables -A INPUT -m mac --mac-source A8:6B:AD:0F:3F:FF -p all -j ACCEPT -m comment --comment "刘正国，192.168.199.78"
iptables -A INPUT -m mac --mac-source 78:2b:cb:04:7d:d0 -p all -j ACCEPT -m comment --comment "服务器，192.168.199.204"
iptables -A INPUT -p icmp --icmp-type 8 -s 0/0 -j DROP -m comment --comment "禁止PING"
iptables -P INPUT DROP 
iptables -P OUTPUT ACCEPT 
iptables -P FORWARD DROP 
service iptables save
service iptables restart
iptables -L -n
```