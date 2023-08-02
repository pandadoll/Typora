[toc]

## IMSI unknown in HSS

![image-20230712171230092](../picture/image-20230712171230092-1690944295950-9-1690945210272-17.png)

解决方案：

1.未在HSS添加卡号

2.添加了重复的卡号

3.卡号已存在，可能是HSS的问题，可能需要重启



## UE identity cannot be derived by the network

![image-20230802155653803](../picture/image-20230802155653803.png)

解决方案：

1.在HSS数据库中查找SIM卡的卡号是否存在

2.HSS自身问题



## Attach reject-Illegal UE

![image-20230802155735799](../picture/image-20230802155735799.png)

![image-20230802160016561](../picture/image-20230802160016561.png)

HSS中msisdn和secretkey的配置问题

解决方案：

msisdn需配置不同，secretkey与上一个配置保持一致



