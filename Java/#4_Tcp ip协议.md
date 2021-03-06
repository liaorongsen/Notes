﻿#### TCP/ip协议

一. TCP/IP 分层


思维导图 : 


TCP / IP 四层简介 : 
-- 应用层 : Telnet, FTP, SMTP, SNMP, DNS, URI, HTML, HTTP 等协议;
-- 运输层 : TCP, UDP, SCTP, DCCP, UDP-Lite 协议;
-- 网络层 : IP, ICMP, IGMP, ARP 协议;
-- 链路层 : 设备驱动程序 与 网络接口 (网卡);


1. 链路层 (数据链路层 / 网络接口层)

链路层 (数据链路层 / 网络接口层) 简介 : 
-- 驱动程序 : 链路层包括 网络相关的设备 的驱动程序, 驱动程序是操作系统与硬件之间进行交互的软件, 相当与桥梁;  
-- 网卡 : 一台计算机有多个网卡 例如 eth0, wlan0 等;  
-- 作用 : 该层主要处理与物理传输介质 (电缆, 光线, 无线) 相关的 物理接口细节相关问题;  
-- 链路层提供者 : 链路层功能一般由 硬件 (网络设备) 及其 驱动程序 提供;


2. 网络层 (互联网层)


(1) 网络层简介

网络层 (互联网层) 简介 : 
-- 作用 : 处理分组在网络中的活动, 如包传输的相关活动 分组选路, 通过该层, 传输层及以上的层次可以抽象甚至忽略网络结构细节;
-- 包含的协议 : IP协议 (网际协议), ICMP 协议 (Internet 互联网控制报文协议), IGMP 协议 (Internet 组管理协议);
-- 网络层提供者 : 网络层 和 传输层的功能 一般由 操作系统 提供;

网络层相关设备分析 : 
-- 实现网络层的设备 : 互联网 就是 实现了 网络层 功能的网络, 互联网的所有 主机 和 路由器 必须实现 IP 功能, 路由器 必须实现通过互联网层 转发分组数据包的功能;
-- 无须实现网络层设备 : 对于其它的网络设备 网桥, 中继器, 集线器 就不用实现 IP 或者  TCP 功能;


(2) IP 协议

IP 协议 : 该协议 基于 IP 地址 转发分包数据;
-- 使用 IP 地址标识 : 该协议 跨网络传送数据包, 整个互联网可以通过 IP 地址作为主机的标识, 能够发送接收数据;
-- IP 协议隐含功能 : IP 协议对上层透明, 该协议隐含 数据链路层 功能, 相互通信的主机 不管经过了 什么种类的数据链路层, 都可以进行通信;
-- 协议可靠性 : IP 协议是分组交换协议, 但是不具备重发机制, 分组数据包未打到目的主机也不会重发, 该协议属于非可靠性传输协议;


(3) ICMP 协议

ICMP 协议 : IP 协议附属协议, 用于主机 和 路由器间交换错误报文 和 其它重要信息;
-- 作用 : 当 IP 数据包在发送过程中无法到达目标主机时, 需要给发送端发一个异常通知, 就是 ICMP 数据包;
-- 使用场景 : 诊断网络的健康状况;


(4) ARP 协议 和 RARP 协议

ARP 协议 : 
-- 作用 : 从分组数据包 IP 地址解析物理地址 (MAC 地址) 协议;

RARP 协议 : 逆地址解析协议;


(5) IGMP 协议

IGMP 协议 (Internet 组网管理协议) : 把一个 UDP 数据报多播到多个主机;



3. 运输层


(1) 运输层简介

运输层简介 : 
-- 作用 : 为两台主机上的应用程序提供端到端的通信, 通过 端口号 识别两台主机上哪些程序在跟哪些程序进行通信; 
-- 包含的协议 : TCP 协议 (传输控制协议), UDP 协议 (用户数据报协议); 


(2) TCP 协议

TCP 协议 (传输控制协议) : 面向链接协议, 该协议为两台主机提供可靠的数据通信; 
-- 分块传网络层 : 在 运输层 中将 应用层 传下来的数据分成数据块; 
-- 确认分组 : 确认接收到数据的分组; 
-- 设置超时时间 : 为数据块设置超时时间, 一旦超过超时时间就重新发送; 

TCP 协议优点 和 局限性 : 
-- 优点 : 保证两个主机间通信可达, 能正确处理在传输过程中丢包, 乱序等异常情况, 其能够有效利用带宽, 缓解网络拥堵;
-- 局限 : TCP 连接建立 和 断开 至少需要 7次发包 和 收包, 导致流量浪费, TCP 中定义的各种为了提高网络效率的规范 不理于视频, 音频传输;


(3) UDP 协议

UDP 协议 (用户数据报协议) : 无连接协议;
-- 作用 : 提供服务简单, 只将数据报分组发送到另一台主机, 不关心数据报能否到达; 
-- 使用场景 : 分组数据较少, 多播, 广播, 多媒体领域;


(4) TCP 与 UDP 比较

TCP 和 UDP 比较 : 
-- TCP : TCP 协议提供了高可靠性的数据通信, 应用层可以忽略数据传输可靠性的细节; 
-- UDP : 使用该协议 必须在应用层考虑数据可靠性; 


4. 应用层


(1) 应用层简介

应用层简介 : 
-- 作用 : 处理应用程序中的细节; 
-- 包含的协议 : Telnet 远程登录, FTP 文件传输协议, SMTP 简单邮件传输协议, SNMP 简单网络管理协议; 


(2) HTTP 协议 

HTTP 协议 : 该协议是 WEB 通信中的 浏览器 与 服务器 端通信所用的协议, 传输数据的格式是 HTML;
-- 协议所在层 : HTTP 属于 WWW 协议中的一种, HTTP 协议对应 OSI 七层协议中的 表示层协议, 在 TCP/IP 协议中属于 应用层协议;


(3) FTP 协议 (文件传输协议)

FTP 协议简介 : 
-- 传输过程 : FTP 协议文件传输会建立两个 TCP 连接, 一个是 传输请求时需要的控制连接, 一个是传输实际文件数据用的数据连接;


(4) TELNET 和 SSH 协议 (远程登录协议)

TELNET 协议 和 SSH 协议 : 这两种协议是 TCP/IP 网络中常用的两种远程登录协议;


(5) SNMP 协议 (网络管理协议)

SNMP 协议介绍 : 
-- 具体的管理过程 : 通过 网络管理终端 (SNMP 管理器) 管理 服务器, 路由器 交换机等, 管理内容有 信息通知, 故障通知, 通知设置, 操作检查, 配置变化 等;
-- 主要作用 : SNMP 协议可以对网络上的设备进行远程修改配置, 可以对大范围复杂网络进行有效管理, 检查 SNMP 代理 是否正常运行, 可以让管理员及时发现故障;

SNMP 协议组成 : 
-- SNMP 代理 (Agent) : SNMP 管理的 路由器, 服务器, 交换机, PC机 等设备叫做 SNMP 代理;
-- 管理器 (Manager) : SNMP 中进行管理的那部分, 即网络管理终端;

MIB 协议 (Manager Infomation Base) : SNMP 代理端 (即 主机, 路由器, 交换机) 保存 网络接口信息, 通信数据量, 异常数据量 以及 设备温度 等信息, 访问这些数据的协议是 MIB 协议;


5. 每层之间的关系

示例 : 网页浏览, 在浏览器端浏览网页;
-- 应用层 : 使用 HTTP 协议;
-- 运输层 : 使用 TCP 协议;
-- 网络层 : 使用 IP 协议;
-- 链路层 : 网络设备驱动程序, 和网卡物理接口;

协议执行位置 : 
-- 用户空间 : 应用层协议 一般是在用户进行中处理, 运行在用户空间;
-- 内核空间 : 运输层 网络层 链路层 一般运行在内核空间中;

下层协议对上层透明 : 高层的协议不关心 也不处理 低层协议 相关数据, 对下层协议一无所知;


5. 网络层 和 运输层的区别

TCP/IP 四层协议的作用 和 区别 : 
-- 链路层 和 应用层 的明显作用 : 链路层处理物理介质相关的传输媒介 以及物理设备相关的细节, 应用层 处理与应用程序相关的数据;
-- 网络层 和 运输层 区别不明显 : 两层主要是网络传输相关, 这两层区分并不是很明确;

不同类型网络连接 : 以太网, 令牌环网, FDDI (光线分布式数据接口), 点对点链接, 这些网络中的主机互相都可以通信;
-- 路由器 : 路由器可以将多个不同类型的网络连接起来;
-- 路由器接口 : 路由器必须有多个网络接口, 

网桥与路由器区别 : 
-- 网桥 : 网桥 在链路层 连接网络; 
-- 路由器 : 路由器 是在网络层连接网络;

端系统 和 中间系统 : 网络层是个中间系统, 传输层是个端系统;
-- 端系统 (End System) : 应用层 和 传输层 使用端到端 (End-to-end) 协议;
-- 中间系统 (Intermediate System) : 网络层 使用逐跳 (Hop-by-hop) 协议;

网络层 和 运输层作用 : 传输层 和 网络层负责的功能不同;
-- 网络层 : IP 协议提供不可靠服务, 将分组从源节点 送到 目的节点;
-- 传输层 : TCP 协议采用超时重传 和 端到端分组确认等机制, 保证可靠性;


二. TCP / IP 相关网络知识


1. 互联网地址


(1) 按照 IP 地址格式分类

IP 地址分类 : IP 地址长度 32bit, 4 个字节, 每个 IP 地址都由 标识位, 网络号, 和 主机号三部分组成;
-- 点分十进制 : 32 位二进制数, 每个十进制整数对应一个字节;
-- 网络区分 : 第一个十进制整数即可区分 IP 地址类型, 0 ~ 127 A类, 128 ~ 191 B类, 192 ~ 223 C类, 224 ~ 239 D类, 240 ~ 225 E类; 

A类地址 : 
-- 组成 : 标识位 0, 网络号 7 位, 主机号 24 位, 
-- 十进制取值范围 : 0.0.0.0 ~ 127.255.255.255; 
-- 二进制取值范围 : 0(标识位) 0000000(网络号) 00000000 00000000 00000000(主机号) ~ 0(标识位) 1111111(网络号) 11111111 11111111 11111111(主机号);

B类地址 : 
-- 组成 : 标识位 10, 网络号 14 位, 主机号 16 位;
-- 十进制取值范围 : 128.0.0.0 ~ 191.255.255.255;
-- 二进制取值范围 : 10(标识位) 000000 00000000(网络号) 00000000 00000000(主机号) ~ 10(标识位) 111111(网络号) 11111111 11111111 11111111(主机号);

C类地址 : 
-- 组成 : 标识位 110, 网络号 21位, 主机号 8位;
-- 十进制取值范围 : 192.0.0.0 ~ 223.255.255.255;
-- 二进制取值范围 : 110(标识位) 00000 00000000 00000000(网络号) 00000000(主机号) ~ 110(标识位) 11111 11111111 11111111(网络号) 11111111(主机号)

D类地址 : 
-- 组成 : 标识位 1110, 多播组号 28 位;
-- 十进制取值范围 : 224.0.0.0 ~ 239.255.255.255;
-- 二进制取值范围 : 1110(标识位) 0000 00000000 00000000 00000000 00000000(多播组号) ~ 1110(标识位) 1111 11111111 11111111 11111111(多播组号);

E类地址 : 
-- 组成 : 标识位 11110, 保留 27 位;
-- 十进制取值范围 : 240.0.0.0 ~ 255.255.255.255;
-- 二进制取值范围 : 11110(标识位) 000 00000000 00000000 00000000(保留位) ~ 11110(标识位) 111 11111111 11111111 11111111(保留位);


(2) 按照 目的地址 分类

按目的地址分类 IP 地址 : 
-- 单播地址 : 目的端为单个主机;
-- 广播地址 : 目的端是给定网络的所有主机;
-- 多播地址 : 目的端是同一组的所有主机;


(3) IP 地址管理机构

互联网络信息中心 (Internet Network Information Center) 简称 InterNIC, 职责是 分配网络号;


2. 域名

DNS (域名系统) : DNS 是一个分布式数据库, 提供 IP 地址 和 主机名之间的映射;

地址解析函数 : 
-- 正函数 : 给定主机名, 查询主机 IP 地址;
-- 逆函数 : 给定主机 IP 地址, 查询主机名;


3. 端口号

端口号简介 : 端口号是 TCP UDP 协议识别主机上的应用程序用的;
-- 端口号位数 : 端口号是 16 bit 的数字, 取值范围 1 ~ 65535;
-- 端口号所在层 : 端口号是 运输层 使用的, 主要配合 TCP UDP 使用, 脱离这两个协议端口号将毫无用处;

客户端口号 (临时端口号) : TCP/IP 提供给客户端的临时端口号是 1024 ~ 5000;
-- 临时端口不重要 : 客户端对临时端口号不关心, 保证端口号唯一即可;

服务端口号 : 
-- 端口号定义位置 : 在 UNIX 系统的文件的 /etc/services 包含了系统服务的端口号, 使用 sudo vim /etc/services 命令查看 Ubuntu 系统的端口号;
-- 服务端口号规律 : TCP/IP 提供的服务端口号都使用的 知名端口号, 即 1 ~ 1023 之间;
-- 服务端口号示例 :  FTP 服务 TCP 端口号 : 21, Telnet 服务 TCP 端口号 : 23, TFTP 服务 UDP 端口号 : 69;
[plain] view plain copy
tcpmux      1/tcp               # TCP port service multiplexer  
echo        7/tcp  
echo        7/udp  
discard     9/tcp       sink null  
discard     9/udp       sink null  
systat      11/tcp      users  
daytime     13/tcp  
daytime     13/udp  
netstat     15/tcp  
qotd        17/tcp      quote  
msp     18/tcp              # message send protocol  
msp     18/udp  
chargen     19/tcp      ttytst source  
chargen     19/udp      ttytst source  
ftp-data    20/tcp  
ftp     21/tcp  
fsp     21/udp      fspd  
ssh     22/tcp              # SSH Remote Login Protocol  
ssh     22/udp  
telnet      23/tcp  
smtp        25/tcp      mail  



三. TCP/IP 分层通信处理过程


1. 数据包首部信息


(1) 首部信息结构

首部信息结构 : TCP/IP 中每个分层都会对数据附加一个首部, 首部包含了该层的相关信息;
-- 传输层 : (TCP 包首部)(TCP 中的数据) ;
-- 网络层 : (IP 包首部)( IP 数据 : (TCP 包首部)(TCP 中的数据)) ;
-- 链路层 : (以太网 包首部) ( 以太网数据 : (IP 包首部)( IP 数据 : (TCP 包首部)(TCP 中的数据))) ;


(2) TCP/IP 数据相关概念

包 : TCP / IP 协议中, 每一层的数据都可以成为包, 是一个全能术语;
帧 (以太网数据帧) : 链路层包的单位;
数据包 (IP数据报) : 网络层以上包的单位, IP 给 链路层的数据包称为 IP 数据报 (IP datagram);
段 (TCP 报文段) : TCP 数据包, TCP 传递给 IP 的数据包成为 TCP 报文段 (TCP segment);
消息 : 应用层的数据包;


2. 封装 和 分用


(1) 封装

以太网数据帧 : 链路层的 数据帧长度在 46 ~ 1500 字节之间;

每个首部的大小 : 以太网首部 | IP 首部 | TCP 首部 | 应用数据(Appl | 用户数据) | 以太网尾部 ;
-- 以太网首部 : 14字节, 需要发送 接收 IP ARP RARP 数据, 上面三种数据包需要标识;
-- IP 首部 : 20字节, 该首部 有一个 8bit 的数值, 用于标识 TCP(标识6) UDP(标识17) ICMP(标识1) IGMP(标识2) 协议; 
-- TCP 首部 : 20字节, 这里也可以是 UDP 首部, 由于 UDP 首部是不可靠的, 数据量比较少, 只有 8字节, TCP 和 UDP 需要 16bit 的端口号标识应用程序;


(2) 分用

分用定义 : 主机在链路层接收到 以太网数据帧, 该数据会逐层上升, 在每层都去掉该层的首部信息, 并校验首部信息是否正确, 该过程就是分用;


(3) 封装 和 分用 总结

封装分用总结 : 发送数据 从应用层到链路层 逐层加数据包 是封装, 接收数据 从链路层到应用层 逐层解包 是分用;



四. TCP / IP 通信示例


1. 数据包发送过程


(1) 应用层处理

准备数据 : 应用层经过用户应用程序处理, 准备要发送的信息;


(2) 传输层处理 (TCP模块)

TCP 连接管理 : 应用层发送数据时, TCP 模块建立连接, 使用这个连接发送数据, 发送完毕之后断开连接;

封装 TCP 首部 : 为了实现可靠的 TCP 传输, 需要在应用层的信息的前端附加一个 TCP 首部;
-- TCP 首部组成 : 源端口号, 目的端口号, 序号, 校验和;


(3) 网络层处理 (IP 模块)

封装 IP 首部 : TCP 模块将 TCP首部 和 TCP 数据合成 TCP 报文段, 传入到 IP 模块, IP 模块在 TCP 报文段前端加上 IP 首部;
-- IP 首部组成 : 接收端 IP 地址, 发送端 IP 地址, 紧随其后的是 UDP/TCP 协议识别标识;

送到链路层 : 
-- 查找路径 : IP 数据报 生成后, 参考路由控制表决定该 IP 数据报的 路由 或 主机 路径;
-- 查询 MAC 地址 : 利用 ARP 查找 MAC 地址;
-- 提交到链路层 : IP 数据报提交给链路层 需要将 MAC 地址 和 IP 地址一起提交到下一层;


(4) 链路层处理

封装 以太网 首部 : 网络层传递 IP 数据报到链路层, 在该层给 IP 数据报附加 以太网首部 并发送出去;
-- 以太网首部组成 : 发送端 MAC 地址, 接收端 MAC 地址, 标识以太网类型(PPP, 令牌环等)的以太网数据协议;

封装 以太网 尾部 : 封装完首部之后, 还需要加上一个 FCS 尾部, 该 FCS 由硬件计算生成, 判断是否因为物理介质干扰而导致数据被破坏;


(5) 包首部添加流程

添加流程 : 应用层信息 -> TCP (UDP) 包首部 -> IP 包首部 -> 以太网包首部 -> 以太网包尾部;

包首部信息规律 : 每层添加的包首部 一般都包含 发送 和 接收 地址信息, 上一层的协议类型;
-- 链路层 : 使用 发送端 和 接收端的 MAC 地址;
-- 网络层 : 使用 发送段 和 接收端的 IP 地址, 该层还需要一个 协议类型, 说明在传输层使用的是 TCP 还是 UDP 协议;
-- 传输层 : 使用 发送段 和 接收端的 端口号;


2. 数据包接收过程


(1) 链路层处理

链路层处理流程 : 判定地址有效行 -> 确定上层协议类型 -> 传递给上层对应协议处理模块;
-- 判定地址有效性 : 主机收到 以太网数据帧后, 解析以太网包头, 查看 目的端 MAC 地址是否为本机地址, 如果不是丢弃该 帧;
-- 判定协议类型 : 查找 以太网数据包首部中的 类型域, 确定该数据包是 IP 数据报, 还是 ARP 协议, 如果无法识别协议类型, 丢弃;
-- 传递给对应模块处理 : 如果是 IP 数据报, 传递给 IP 模块处理, 如果是 ARP 协议, 就传递给 ARP 模块处理;


(2) 网络层处理 (IP 模块)

IP 模块处理流程 (没有路由器的情况) : 判定 IP 地址 -> 判定协议;
-- 判定 IP : 分析 IP 包首部的 目的 IP 地址, 如果 IP 地址是本机地址, 就进行下面的操作;
-- 判定协议 : 判定 上层协议是 TCP 还是 UDP,  交给对应模块处理;

有路由器的情况 : 如果有路由器, 接收端地址一般是 路由器的 IP 地址, 需要查询路由表, 查询到 目的主机 或 路由器 后在转发数据包;


(3) 传输层处理 (TCP 模块)

传输层处理流程 : 校验数据完整性 -> 校验数据次序 -> 校验程序端口 -> 发送回执 -> 送到应用层;
-- 校验完整性 : TCP 模块接收到数据包后, 首先计算校验和, 判断数据是否被破坏;
-- 校验次序 : 检查数据包的序号, 查看是否在按照次序接收数据;
-- 校验端口号 : 通过端口号获取具体的应用程序;
-- 发送回执 : 数据接收完毕后, 发送一个回执给发送端, 如果发送端未收到回执, 发送端会一直发送数据给接收端;
-- 传递给应用层 : 数据被接收以后, 会根据端口号识别应用程序, 将数据传递给那个应用程序;