# 概述

8088模组中 GPS/GPRS 之间的切换通过 SW 命令实现。

# SW命令的使用

## sw=1

软件由gprs模式切换到gps模式。如果当前gprs处于gmm注册状态，则此功能将隐式执行去附着指令，待完成后再切换到gps模式，如果当前gprs未注册到网络，则直接切换到gps模式；

## sw=2

软件由gps模式切换到gprs模式。此指令将直接结束gps任务，将软件切换到gprs模式；

# 使用演示

下图是通过串口工具发送sw指令以及AT指令进行演示说明（注：在发送命令后跟“\r\n”）。

**GPRS —> GPS**

<img src=".\png\GPRS-GPS.png" style="zoom:80%;" />  

**GPS —> GPRS**

<img src=".\png\GPS-GPRS.png" style="zoom:80%;" />  

