（一）X2建立

**FGSG2-2198** [6.X]X2 abnormal if delete neighbor when building the X2 setup connection

- 现象：

  基站添加邻区后，L3 log没有打印建立X2的信令

![image-20230721152851867](E:\Typora\picture\image-20230721152851867-1690944275310-1.png)

​		从oamv1中观察到两个站X2建立情况异常

![image-20230721152629898](E:\Typora\picture\image-20230721152629898-1690944284191-3.png)

- 正常建立X2的流程

1. 基站添加邻区后，会向MME询问对方站的IP地址
2. 待MME回复消息后，基站更新对方站的IP地址
3. 待更新完后，基站先与对方站建立SCTP连接，再建立X2连接

- L3信令流程（源站）：

[20230721 07:04:47.440586] [Info]    [SON ANR] Added neighbour cell 257949704 (OAM)
[20230721 07:04:47.440615] [Info]    [L3] add eutra neighbour cell_id = 257949704
[20230721 07:04:47.440629] [Info]    [L3] start dynamic configure timer
[20230721 07:04:47.440749] [Info]    [SON ANR] Added neighbour eNB 257949704 (OAM)
[20230721 07:04:47.443478] [Info]    [eNB]->[MME 1] ENB Configuration Transfer
[20230721 07:04:47.443632] [Info]    002840550000010081404e4064f01040f600008064f010223d0064f01040f600013064f010223d0000000098402b47f00020004000000000000000000000000f0000009940130203f80020004000000000000000000000000f
[20230721 07:04:47.443722] [Info]    [ALARM] CLEAR, Cellid(0), Event id(136), Information(PCI CONFUSION: 501,1,0, cellid: 0)
[20230721 07:04:47.443750] [Info]    [L3] Success received for Dynamic configuration
[20230721 07:04:47.456864] [Info]    00294067000001008240604064f01040f600013064f010223d0064f01040f600008064f010223d503f800020004000000000000000000000000700000098402b47f0002000400000000000000000000000070000009940130203f800200040000000000000000000000007
[20230721 07:04:47.456883] [Info]    [MME 1]->[eNB] MME Configuration Transfer
[20230721 07:04:47.457086] [Info]    [L3] Update eNB [257949704] From MmeConfigurationTransfer
[20230721 07:04:47.457130] [Info]    [SON ANR] Updated neighbour eNB 257949704 (MME Configuration Transfer)
[20230721 07:04:47.457597] [Info]    [X2] Attempting to connect to eNB 257949704
[20230721 07:04:47.462547] [Info]    [X2] Failed X2 connection establishment (1/3) with eNB 257949704: timeout waiting X2 connection confirmation
[20230721 07:04:47.462566] [Info]    [X2AP] Scheduled X2 connection establishment reattempt with eNB 257949704 in 2000 ms
[20230721 07:04:47.475526] [Info]    [L3] allcount = 0, dynupdate.cellCallNum = 0, dynupdate.enbCallNum = 0
[20230721 07:04:47.878769] [Info]    [Cell 1] SIB1
[20230721 07:04:47.878788] [Info]    68d180039884482447bec00027250d7f190208184302246c0f01c01200000000
[20230721 07:04:47.878845] [Info]    [Cell 1] SIB5
[20230721 07:04:47.878860] [Info]    100eb6043f9ba800028f0300c46c146e375000041e060188ddfffe6ea0000a3c3e6f0300c407800a09d47e80e48809009b000000000000
[20230721 07:04:49.440109] [Info]    [X2] Attempting to connect to eNB 257949704
[20230721 07:04:49.448362] [Info]    [SCTP] Connection established with 20:40::7
[20230721 07:04:49.448551] [Info]    [X2] Connection established with eNB 257949704 (20:40::7)
[20230721 07:04:49.448557] [Info]    [eNB]->[eNB 257949704] X2 Setup Request
[20230721 07:04:49.448599] [Info]    0006007d000003001500090064f01040f600013000140059004801f50064f010f600013223d264f01013100210ffffffff550001005f0004400104100060000320041000010029400140003740064000011050c000014064f010f600008001f3ffff0001004c4002223d005e00032004d80018000c1013100200010064f0100001
[20230721 07:04:49.455081] [Info]    2006008257000003001500090064f01040f6000080001400822c004801f30064f010f600008223d464f01013100213410810ffffffff550001005f0004400104d8006000032004d800010029400140003740064000001050c000144064f010f60003e001ebffff0001004c4002223d005e00032004d84064f010f600020001f1ffff0001004c4002223d005e00032004d84064f010f600007001e0ffff0001004c4002223d005e00032004d84064f010f600006001e3ffff0001004c4002223d005e00032004d84064f010f600002001f6ffff0001004c4002223d005e00032004d84064f010003e808001eeffff0001004c4002223d005e00032004d84064f010f60002c001e5146e0000004c4002223d4064f010f60000c001f4146e0000004c4002223d4064f010f60000a001e8146e0000004c4002223d4064f010f600004001ef146e0000004c4002223d4013100201a6c0a0008a146e0000004c400206244064f010f60000f001f3146e0000004c4002223d4064f010f600018001e8146e0000004c4002223d4064f010f600015001ebffff0001004c4002223d005e00032004104064f010f600013001f5ffff0001004c4002223d005e00032004104064f0101b277d4001edffff0001004c4002223d005e00032004104064f0101b2761e001ecffff0001004c4002223d005e00032004104064f010003e814001e2ffff0001004c4002223d005e00032004104064f010003e810001f6ffff0001004c4002223d005e000320041040131002f600014001e8ffff0001004c4002223d005e00032005dc001800122013410800010013100200010064f0100001
[20230721 07:04:49.455762] [Info]    [eNB 257949704]->[eNB] X2 Setup Response
[20230721 07:04:49.456509] [Info]    [ALARM] stack alarm : CLEAR, Cellid(1) , id(11110) ,Information (X2 Setup fails)
[20230721 07:04:49.456597] [Info]    [SON ANR] Updated neighbour eNB 257949704 (X2 Setup)
[20230721 07:04:49.456606] [Info]    [SON ANR] Updated neighbour cell 257949704 (X2 Setup)
[20230721 07:04:49.456653] [Info]    [Neighbour Management] Updated eNB 64F010-257949704 (X2 Setup)

- 抓包观察SCTP和X2连接建立过程：

![image-20230721152218068](E:\Typora\picture\image-20230721152218068-1690944289405-5.png)



（二）功率

**NRSTACK-658** LTE Reference Signal Power is error when the bandwidth is changed from 5M to 20M.

- 现象：基站从BW 5M，Reference Signal Power -4修改到BW 20M后，Reference Signal Power维持-4不变，txpower（30）超出范围（最大24dBm）

![image-20230721154226036](E:\Typora\picture\image-20230721154226036-1690944290925-7.png)

- 分析：

  - Tx Power是根据Reference Signal Power计算后显示的，所以原因出在Reference Signal Power上

  - 带宽从5M修改到20M，Reference Signal Power仍显示-4，不合理

    

- 计算公式：

单天线的发射功率：

txPower_4G = recommend_reference_signal_power+ 10 * lg(dl_bandwidth* 12)

双天线的发射功率：

txPower_4G = recommend_reference_signal_power+ 10 * lg(dl_bandwidth* 12)**+3**

【dl_bandwidth为对应带宽的RB数，如20M带宽则RB数为100】



一般来说，基站各带宽的最大的参考信号发射功率如下表：

<table>
    <tr>
        <td>带宽</td>
        <td>参考信号功率值</td>
        <td>每个天线发射功率(dBm)</td>
        <td>总发射功率(dBm)</td>
        <td>Watt</td>
    </tr>
    <tr>
        <td rowspan="2">20MHz</td>
        <td>-10</td>
        <td>21</td>
        <td>24</td>
        <td>250mW</td>
    </tr>
    <tr>
        <td>-23</td>
        <td>8</td>
        <td>11</td>
        <td>13mW</td>
    </tr>
        <tr>
        <td rowspan="2">15MHz</td>
        <td>-9</td>
        <td>21</td>
        <td>24</td>
        <td>250mW</td>
    </tr>
    <tr>
        <td>-22</td>
        <td>8</td>
        <td>11</td>
        <td>13mW</td>
    </tr>
    <tr>
        <td rowspan="2">10MHz</td>
        <td>-7</td>
        <td>21</td>
        <td>24</td>
        <td>250mW</td>
    </tr>
    <tr>
        <td>-20</td>
        <td>8</td>
        <td>11</td>
        <td>13mW</td>
    </tr>
        <tr>
        <td rowspan="2">5MHz</td>
        <td>-4</td>
        <td>21</td>
        <td>24</td>
        <td>250mW</td>
    </tr>
    <tr>
        <td>-17</td>
        <td>8</td>
        <td>11</td>
        <td>13mW</td>
    </tr>
</table>



- **W,dBW和dBm转换计算器**的网址：

https://tool.520101.com/rf/dbmzhuanhuan/



