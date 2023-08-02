[TOC]

## 1.使用bat文件进行抓包

### a.存放plink3.exe文件到指定目录

> C:\Windows\System3
>
> C:\Windows\SysWOW64

### b.执行plink3命令登录基站

执行plink抓包命令前先在命令行cmd执行以下命令（按提示操作登录成功后，ctrl c退出登录，已经成功登录过一次就不用此步操作）

> plink3 基站IP

示例截图：

![image-20230712164016675](E:\Program Files\Typora\picture\image-20230712164016675.png)

### c.创建bat文件，写入参数

首先创建一个txt文件，写入以下内容，完成后修改文件后缀为bat，双击文件即可开始抓包

> set user=root
> set enb_pwd="Ca$a@pex!234"
> set gnb_pwd="VzWNetExtender4"
> set gnb_ip="172.0.18.115"
> set enb_ip="172.0.18.110"
> set cap_filename=cap_test_1.pcapng
> set wireshark_position=E:\\Program Files\Wireshark\Wireshark.exe
> set range="sctp"
>
> 
>
> :: 双模站
> plink3.exe -ssh -batch -pw %gnb_pwd% %user%@%gnb_ip% "/usr/local/bin/tcpdump -i any -w - %range%" | "%wireshark_position%" -k -i - -b packets:300000 -w E:\wireshark_log\%cap_filename%
>
> 
>
> :: 单模
> ::plink3.exe -ssh -batch -pw %enb_pwd% %user%@%enb_ip% "/usr/sbin/tcpdump -i any -w - %range%" | "%wireshark_position%" -k -i - -b packets:300000 -w E:\wireshark_log\%cap_filename%

注：

- ::	表示注释
- -b	表示抓到300000个数据包即完成一个文件
- range	表示过滤规则，当为==空格==时，则抓全部的包
- wireshark_position和E:\wireshark_log\%cap_filename%需修改成自己电脑对应的位置



## 2.其他异常情况

出现以下异常情况时，将命令中的==“-batch”去掉==，在cmd执行一次

![image-20230712165500508](E:\Program Files\Typora\picture\image-20230712165500508.png)

在此处写入y，成功后退出

![image-20230712165812442](E:\Program Files\Typora\picture\image-20230712165812442.png)

之后可以正常执行抓包命令抓包

命令样本：

> plink3.exe -ssh -pw "VzWNetExtender4" root@172.0.19.59 "/usr/local/bin/tcpdump -neli any -s 0 -w - sctp" | "E:\Program Files\Wireshark\Wireshark.exe" -k -i -