[toc]

## 1.查看已接入的UE数量以及已经接入的UE数量

> show pgw-c pool all

![image-20230808111915444](../picture/image-20230808111915444.png)



## 2.根据IMSI查找UE的详细信息（APN-AMBR-QCI）

> show sgw-c session imsi 460012345678890

![image-20230808112018845](../picture/image-20230808112018845.png)



## 3.从saegw处发起UE Detach的效果

> clear sgw-c session imsi 460012345678890

