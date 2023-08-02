[TOC]

## 常用抓包命令

> tcpdumo -i any sctp -w test.pcap

只抓sctp的包

> tcpdump -i any '(host 172.0.18.135 and esp)' or '(host 20:40::f and sctp)' -c 50 -w test.pcap

使用**==括号==**组合多个过滤器（括号在shell中是特殊符号，需使用==**引号**==将其包含【单引号双引号都可】）

-c	指明抓多少个数据包

> tcpdumo -i any port 319 or port 320 -w ptp.pcap
>

抓取与ptp服务器交互的数据包（在基站或者ptp服务器上执行）



