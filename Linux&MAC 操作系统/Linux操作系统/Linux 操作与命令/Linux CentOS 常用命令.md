* # 推荐网站
  * [linux命令手册](http://linux.51yip.com/)
* # 常用命令分类
  * ### 安装和登录命令
    > #### shutdown、halt、reboot、install、mount、umount、exit、last、yum、rpm、configure、make
  * ### 文件处理命令
    > #### file、mkdir、grep、dd、find、mv、ls、diff、cat、ln、tar、unzip、gunzip、unarj
  * ### 系统管理相关命令
    > #### df、top、free、quota、at、lp、adduser、groupadd、kill、crontab
  * ### 网络操作命令
    > #### ifconfig、ip、ping、netstat、telnet、ftp、route、rlogin、rcp、finger、mail、 nslookup
  * ### 系统安全相关命令
    > #### passwd、su、umask、chgrp、chmod、chown、chattr、sudo ps、who、iptables
* # 常用功能命令解说
  * ### 查看Linux内核版本
    ```shell
    uname -a
    uname -r
    ```
  * ### 查看Linux系统版本
    ```shell
    lsb_release -a
    more /etc/redhat-release
    cat /etc/redhat-release
    ```
  * ### 查看硬件cpu
    ```shell
    more /proc/cpuinfo | grep "model name"
    grep "CPU" /proc/cpuinfo
    ```
  * ### 查看cpu是32位还是64位
    ```shell
    getconf LONG_BIT
    ```
  * ### 查看内存总量
    ```shell
    grep MemTotal /proc/meminfo 
    ```
  * ### 查看空闲内存量
    ```shell
    grep MemFree /proc/meminfo
    ```
  * ### 查看内存占用
    ```shell
    free -m
    ```
  * ### 查看硬盘占用率
    ```shell
    df -h
    ```
  * ### 查看磁盘分区信息
    ```shell
    fdisk -l
    ```
  * ### 查看指定目录磁盘空间占用情况
    ```shell
    du /etc -sh
    ```
  * ### 查看现在已经安装了那些软件包
    ```shell
    rpm -qa
    yum list installed
    ```
  * ### 统计安装了多少个软件包
    ```shell
    rpm -qa | wc -l
    yum list installed | wc -l
    ```
  * ### 查看主机名
    ```shell
    hostname
    ```
  * ### 查看开机运行时间
    ```shell
    uptime
    ```
  * ### 监控当前linux的系统状况
    ```shell
    top
    ```
    ![](https://img-blog.csdn.net/20170803091750608?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzYzNTc4MjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
    * #### 第一行解释：
      > top - 01:18:39 up 2 days, 18:54, 1 user, load average: 0.04, 0.03, 0.05
      
      * 01:18:39：系统当前时间
      * up 2 days, 18:54 ：系统开机到现在经过了2天
      * 1 users：当前1用户在线
      * load average:0.04, 0.03, 0.05：系统1分钟、5分钟、15分钟的CPU负载信息.
        > 备注：load average后面三个数值的含义是最近1分钟、最近5分钟、最近15分钟系统的负载值。这个值的意义是，单位时间段内CPU活动进程数。如果你的机器为单核，那么只要这几个值均<1，代表系统就没有负载压力，如果你的机器为N核，那么必须是这几个值均＜N才可认为系统没有负载压力。

    * #### 第二行解释：
      > Tasks: 108 total, 1 running, 107 sleeping, 0 stopped, 0 zombie
      
      * 108 total：当前有108个任务
      * 1 running：1个任务正在运行
      * 107 sleeping：107个进程处于睡眠状态
      * 0 stopped：停止的进程数
      * 0 zombie：僵死的进程数
    * #### 第三行解释：
      > %Cpu(s): 0.1 us, 0.2 sy, 0.2 ni, 99.4 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st
      
      * 0.1%us：用户态进程占用CPU时间百分比
      * 0.2%sy：内核占用CPU时间百分比
      * 0.2%ni：renice值为负的任务的用户态进程的CPU时间百分比。nice是优先级的意思
      * 99.4%id：空闲CPU时间百分比
      * 0.0%wa：等待I/O的CPU时间百分比
      * 0.0%hi：CPU硬中断时间百分比
      * 0.0%si：CPU软中断时间百分比
    * #### 第四行解释：
      > KiB Mem : 3882172 total, 1079980 free, 1684652 used, 1117540 buff/cache
      
      * 3882172 k total：物理内存总数
      * 1684652k used： 使用的物理内存
      * 1079980k free：空闲的物理内存
      * 1117540k cached：用作缓存的内存
    * #### 第五行解释：
      > KiB Swap: 0 total, 0 free, 0 used. 1871412 avail Mem
      
      * 0k total：交换空间的总量
      * 0k used： 使用的交换空间
      * 0k free：空闲的交换空间
      * 1871412k cached：缓存的交换空间
    * #### 最后一行：
      > PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
      
      * PID：进程ID
      * USER：进程的所有者
      * PR：进程的优先级
      * NI：nice值
      * VIRT：占用的虚拟内存
      * RES：占用的物理内存
      * SHR：使用的共享内存
      * S：进行状态 S：休眠 R运行 Z僵尸进程 N nice值为负
      * %CPU：占用的CPU
      * %MEM：占用内存
      * TIME+： 占用CPU的时间的累加值
      * COMMAND：启动命令
  * ### 用w查看分用户linux负载
    ```shell
    [root@elk-node01 logstash]# w
     01:34:58 up 2 days, 19:10,  1 user,  load average: 0.07, 0.06, 0.06
    USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
    centos   pts/4    192.168.60.107   Tue07    2.00s  0.09s  0.14s sshd: centos [priv] 
    ```
    * #### 第一行参考top命名的第一行解释
    * #### 第二行
      * USER —登录的用户名
      * TTY —登录后系统分配的终端号
      * FROM—远程主机名，即从哪儿登录来的
      * LOGIN@—何时登录
      * IDLE—空闲了多长时间，表示用户闲置的时间。这是一个计时器，一旦用户执行任何操作，该计时器便会被重置
      * JCPU—和该终端（tty）连接的所有进程占用的时间，这个时间里并不包括过去的后台作业时间，但却包括当前正在运行的后台作业所占用的时间
      * PCPU—指当前进程（即在WHAT项中显示的进程）所占用的时间
      * WHAT—当前正在运行进程的命令行
  * ### vmstat查看linux负载
    ```shell
    [root@elk-node01 logstash]# vmstat
    procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
     r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
     1  0      0 1080064    888 1116712    0    0     0     2   31   49  1  0 99  0  0
    ```
    * r 表示运行队列(就是说多少个进程真的分配到CPU)，当这个值超过了CPU数目，就会出现CPU瓶颈。这个也和top的负载有关系，一般负载超过了3就比较高，超过了5就高，超过了10就不正常了，服务器的状态很危险。top的负载类似每秒的运行队列。如果运行队列过大，表示你的CPU很繁忙，一般会造成CPU使用率很高。
    * b 表示阻塞的进程,这个不多说，进程阻塞，大家懂的。
    * swpd 虚拟内存已使用的大小，如果大于0，表示你的机器物理内存不足了，如果不是程序内存泄露的原因，那么你该升级内存了或者把耗内存的任务迁移到其他机器。
    * free 空闲的物理内存的大小，我的机器内存总共8G，剩余3415M。
    * buff Linux/Unix系统是用来存储，目录里面有什么内容，权限等的缓存，我本机大概占用300多M
    * cache cache直接用来记忆我们打开的文件,给文件做缓冲，(这里是Linux/Unix的聪明之处，把空闲的物理内存的一部分拿来做文件和目录的缓存，是为了提高 程序执行的性能，当程序使用内存时，buffer/cached会很快地被使用。)
    * si 每秒从磁盘读入虚拟内存的大小，如果这个值大于0，表示物理内存不够用或者内存泄露了，要查找耗内存进程解决掉。我的机器内存充裕，一切正常。
    * so 每秒虚拟内存写入磁盘的大小，如果这个值大于0，同上。
    * bi 块设备每秒接收的块数量，这里的块设备是指系统上所有的磁盘和其他块设备，默认块大小是1024byte。
    * bo 块设备每秒发送的块数量，例如我们读取文件，bo就要大于0。bi和bo一般都要接近0，不然就是IO过于频繁，需要调整。
    * in 每秒CPU的中断次数，包括时间中断
    * cs 每秒上下文切换次数，例如我们调用系统函数，就要进行上下文切换，线程的切换，也要进程上下文切换，这个值要越小越好，太大了，要考虑调低线程或者进程的数目,例如在apache和nginx这种web服务器中，我们一般做性能测试时会进行几千并发甚至几万并发的测试，选择web服务器的进程可以由进程或者线程的峰值一直下调，压测，直到cs到一个比较小的值，这个进程和线程数就是比较合适的值了。系统调用也是，每次调用系统函数，我们的代码就会进入内核空间，导致上下文切换，这个是很耗资源，也要尽量避免频繁调用系统函数。上下文切换次数过多表示你的CPU大部分浪费在上下文切换，导致CPU干正经事的时间少了，CPU没有充分利用，是不可取的。
    * us 用户CPU时间，我曾经在一个做加密解密很频繁的服务器上，可以看到us接近100,r运行队列达到80(机器在做压力测试，性能表现不佳)。
    * sy 系统CPU时间，如果太高，表示系统调用时间长，例如是IO操作频繁。
    * id 空闲 CPU时间，一般来说，id + us + sy = 100,一般认为id是空闲CPU使用率，us是用户CPU使用率，sy是系统CPU使用率。
    * wt 等待IO CPU时间。
  * ### ps：将某个时间点的程序运行情况撷取下来
    ```shell
    [root@www ~]# ps aux  #观察系统所有的程序数据
    [root@www ~]# ps -lA  #同上，也是能够观察所有系统的数据
    [root@www ~]# ps axjf #连同部分程序树状态
    [root@www ~]# ps –aux | grep 程序名     #列出系统当前运行的进程
    [root@www ~]# ps -l #仅观察自己的 bash 相关程序
    F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
    4 S     0  3477  3475  0  75   0 - 16551 wait   pts/1    00:00:00 bash
    4 R     0  3743  3477  0  78   0 - 15882 -      pts/1    00:00:00 ps
    ```
    * F：代表这个程序旗标 (process flags)，说明这个程序的总结权限，常见号码有：
      * 若为 4 表示此程序的权限为 root 
      * 若为 1 则表示此子程序仅进行复制(fork)而没有实际运行
    * S：代表这个程序的状态 (STAT)，主要的状态有：
      * R (Running)：该程序正在运行中；
      * S (Sleep)：该程序目前正在睡眠状态(idle)，但可以被唤醒(signal)。
      * D ：不可被唤醒的睡眠状态，通常这支程序可能在等待 I/O 的情况(ex>列印)
      * T ：停止状态(stop)，可能是在工作控制(背景暂停)或除错 (traced) 状态；
      * Z (Zombie)：僵尸状态，程序已经终止但却无法被移除至内存外。
    * C：代表 CPU 使用率，单位为百分比；
    * RI/NI：Priority/Nice 的缩写，代表此程序被 CPU所运行的优先顺序，数值越小代表该程序越快被 CPU 运行
    * ADDR/SZ/WCHAN：都与内存有关
      * ADDR 是 kernel function，指出该程序在内存的哪个部分，如果是个 running 的程序，一般就会显示『 - 』
      * SZ 代表此程序用掉多少内存
      * WCHAN 表示目前程序是否运行中，同样的， 若为- 表示正在运行中
    * TTY：登陆者的终端机位置，若为远程登陆则使用动态终端介面 (pts/n)
    * TIME：使用掉的 CPU 时间，注意，是此程序实际花费 CPU 运行的时间，而不是系统时间
    ![](https://img-my.csdn.net/uploads/201112/7/0_13232703116JZT.gif)
      * 内存使用及其VSZ（虚拟内存大小）和RSS（常驻集大小）；
        * VSZ表示一个程序完全驻留在内存的话需要占用多少内存空间；
        * RSS指明了当前实际用了多少内存；
      * STAT显示了进程当前的状态：
        * “S”进程处在睡眠状态，表明这些进程在等待某些事情发生—可能是用户输入或者系统资源的可用性（sleeping）；
        * “D”不可中断Uninterruptible sleep（Usually IO）
        * ”R“正在运行，或在队列中的进程runnable（on run queue）
        * “T”停止或被迫追踪（traced or stoped）
        * “Z”僵尸进程 a defunct （“zombie”process）
        * “W”进入内存交换（从内核2.6开始无效）
        * “X”死掉的进程
        * “L”内存锁
  * ### 查看进程IO使用率
    ```shell
    yum install iotop
    iotop
    iotop  -o #直接查看输出比较高的磁盘读写程序
    ```
  * ### 监控网络连接情况
    ```shell
    列出所有端口 (包括监听和未监听的)
    netstat -a     #列出所有端口
    netstat -at    #列出所有tcp端口
    netstat -au    #列出所有udp端口                             

    列出所有处于监听状态的 Sockets
    netstat -l        #只显示监听端口
    netstat -lt       #只列出所有监听 tcp 端口
    netstat -lu       #只列出所有监听 udp 端口
    netstat -lx       #只列出所有监听 UNIX 端口

    显示每个协议的统计信息
    netstat -s    #显示所有端口的统计信息
    netstat -st   #显示TCP端口的统计信息
    netstat -su   #显示UDP端口的统计信息

    netstat -pt   #显示 PID 和进程名称
    netstat -rn   #显示路由信息
    netstat -aple | grep mysql    #查看服务是否在运行
    netstat -apn |grep 端口号     #扫描端口的进程占用情况
    ```
    * Proto 连接使用的协议
      - t ：TCP
      - u ：UDP
      - raw ：RAW类型
      - unix ：UNIX域类型
      - ax25 ：AX25类型
      - ipx ：ipx类型
      - netrom ：netrom类型
    * RefCnt表示连接到本套接口上的进程号
    * Type显示套接口的类型
    * STATE状态说明
      - LISTEN：侦听来自远方的TCP端口的连接请求
      - SYN-SENT：再发送连接请求后等待匹配的连接请求（如果有大量这样的状态包，检查是否中招了）
      - SYN-RECEIVED：再收到和发送一个连接请求后等待对方对连接请求的确认（如有大量此状态，估计被flood攻击了）
      - ESTABLISHED：代表一个打开的连接
      - FIN-WAIT-1：等待远程TCP连接中断请求，或先前的连接中断请求的确认
      - FIN-WAIT-2：从远程TCP等待连接中断请求
      - CLOSE-WAIT：等待从本地用户发来的连接中断请求
      - CLOSING：等待远程TCP对连接中断的确认
      - LAST-ACK：等待原来的发向远程TCP的连接中断请求的确认（不是什么好东西，此项出现，检查是否被攻击）
      - TIME-WAIT：等待足够的时间以确保远程TCP接收到连接中断请求的确认
      - CLOSED：没有任何连接状态
    * Path表示连接到套接口的其它进程使用的路径名
  * ### 查看指定端口使用情况
    ```shell
    lsof -i:端口号
    ```
  * ### 文件搜索命令find
    ```shell
    find / -name install.log
    ```
  * ### 删除ssh中旧的RSA key
    ```shell
    ssh-keygen -R 192.168.137.254
    ```
  * ### git 免密码，去除每次输入密码
    ```shell
    $ git clone http://github.com/git_dir
    $ vi ~/git_dir/.git/config
    [credential]
          helper = store
    ```
  * ### 利用SSH传输文件或目录
    * #### 从服务器上下载文件
      ```shell
      scp username@servername:远端目录文件 本地目录
      ```
    * #### 上传本地文件到服务器
      ```shell
      scp 本地绝对目录文件 username@servername:远端目录
      ```
    * #### 从服务器下载整个目录
      ```shell
      scp -r username@servername:远程目录 本地目录
      ```
    * #### 上传目录到服务器
      ```shell
      scp  -r 本地目录 username@servername:远程目录
      ```
  * ### 利用`screen`命令进行长时间SSH操作
    * #### 背景
      > 系统管理员经常需要SSH 或者telent 远程登录到Linux 服务器，经常运行一些需要很长时间才能完成的任务，比如系统备份、ftp 传输等等。通常情况下我们都是为每一个这样的任务开一个远程终端窗口，因为它们执行的时间太长了。必须等待它们执行完毕，在此期间不能关掉窗口或者断开连接，否则这个任务就会被杀掉，一切半途而废了。
    * #### 安装
      ```shell
      # yum install screen
      # rpm -qa|grep screen
      screen-4.0.3-4.el5
      ```
    * #### 语法
      ```shell
      screen [-AmRvx -ls -wipe][-d <作业名称>][-h <行数>][-r <作业名称>][-s ][-S <作业名称>]
      ```
      * ##### ` screen` 参数说明
        参数|说明
        -|-
        -A|将所有的视窗都调整为目前终端机的大小。
        -d <作业名称>|将指定的screen作业离线。
        -h <行数>|指定视窗的缓冲区行数。
        -m |即使目前已在作业中的screen作业，仍强制建立新的screen作业。
        -r <作业名称>|恢复离线的screen作业。
        -R |先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
        -s |指定建立新视窗时，所要执行的shell。
        -S <作业名称> |指定screen作业的名称。
        -v |显示版本信息。
        -x |恢复之前离线的screen作业。
        -ls或--list |显示目前所有的screen作业。
        -wipe |检查目前所有的screen作业，并删除已经无法使用的screen作业。
      * #### 在每个screen session 下，所有命令都以 ctrl+a(C-a) 快捷方式进行
        快捷键|说明
        -|-
        C-a ? |显示所有键绑定信息
        C-a c |创建一个新的运行shell的窗口并切换到该窗口
        C-a n |Next，切换到下一个 window 
        C-a p |Previous，切换到前一个 window 
        C-a 0..9 |切换到第 0..9 个 window
        Ctrl+a [Space] |由视窗0循序切换到视窗9
        C-a C-a |在两个最近使用的 window 间切换 
        C-a x |锁住当前的 window，需用用户密码解锁
        C-a d | detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows) 丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里，每个 window 内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。 
        C-a z | 把当前session放到后台执行，用 shell 的 fg 命令则可回去。
        C-a w | 显示所有窗口列表
        C-a t | Time，显示当前时间，和系统的 load 
        C-a k | 杀死当前的窗口，同时也将杀死这个窗口中正在运行的进程。
        C-a [ | 进入 copy mode，在 copy mode 下可以回滚、搜索、复制就像用使用 vi 一样
        
    * #### 常用`screen`的使用
      ```shell
      screen -S yourSessionName       # 新建一个叫 yourSessionName 的 session
      screen -ls                      # 列出当前所有的session
      screen -r yourSessionName       # 回到 yourSessionName这个 session
      screen -d yourSessionName       # 远程 detach 某个 session
      screen -d -r yourSessionName    # 结束当前 session 并回到 yourSessionName 这个 session
      ```
    * #### `screen`会话共享
      假设你在和朋友在不同地点以相同用户登录一台机器，然后你创建一个screen会话，你朋友可以在他的终端上命令：
      ```shell
      screen -x
      ```
      这个命令会将你朋友的终端Attach到你的Screen会话上，并且你的终端不会被Detach。这样你就可以和朋友共享同一个会话了，如果你们当前又处于同一个窗口，那就相当于坐在同一个显示器前面，你的操作会同步演示给你朋友，你朋友的操作也会同步演示给你。
  * ### 修改静态IP
    ```shell
    # cd /etc/sysconfig/network-scripts
    TYPE=Ethernet
    BOOTPROTO=static
    DEFROUTE=yes
    PEERDNS=yes
    PEERROUTES=yes
    IPV4_FAILURE_FATAL=no
    IPADDR=192.168.85.100 #静态IP
    GATEWAY=192.168.85.2 #默认网关
    NETMASK=255.255.255.0 #子网掩码
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_PEERDNS=yes
    IPV6_PEERROUTES=yes
    IPV6_FAILURE_FATAL=no
    NAME=eno16777736
    UUID=ae05ccde-6a29-4332-b486-f3042da73ac0
    DEVICE=eno16777736
    ONBOOT=yes
    
    # service network restart
    ```
  * ### wget的使用技巧
    * #### 镜像一个网站
      > 如果网站中的图像是放在另外的站点，那么可以使用 -H 选项。
      ```shell
      wget -m -k (-H) http://www.example.com/
      ```
    * #### 批量下载
      > 所有需要下载文件的地址放到 filename.txt 中，然后 wget 就会自动为你下载所有文件
      
      ```shell
      wget -i filename.txt
      ```
    * #### 断点续传
      ```shell
      wget -c http://example.com/really-big-file.iso
      ```
    * #### 下载指定扩展名的文件
      > 可以指定多个扩展名，只需用逗号分隔即可
      
      ```shell
      wget -r -np -nd --accept=iso http://example.com/centos-5/i386/
      ```
    * #### 下载指定目录中的所有文件
      > -np 的作用是不遍历父目录
      > -nd 表示不在本机重新创建目录结构
      
      ```shell
      wget -r -np -nd http://example.com/packages/
      ```