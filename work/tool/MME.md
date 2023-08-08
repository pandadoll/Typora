[toc]

## 1.查看已连接的基站的具体信息

> show mme enodebs verbose

![image-20230808104852626](../picture/image-20230808104852626.png)

下图中的f60006f是1007616-111换算来的
基站的cellidentity为1007616*256+111=257949807

![image-20230808104943580](../picture/image-20230808104943580.png)



## 2.查找相连的HSS

> show diameter-endpoint

![image-20230808105104547](../picture/image-20230808105104547.png)

> show running-config | include HSS

- 注：include后严格区分大小写

![image-20230808105153369](../picture/image-20230808105153369.png)



## 3.查看已相连的基站（简洁）

> show mme enodebs list

![image-20230808105313367](../picture/image-20230808105313367.png)