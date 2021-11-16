# GNSS通信协议



本文档主要目的是介绍UCchip-GNSS的串口输出协议，协议主要参考了标准的NMEA0183 v4.11协议。

## 语句通用格式

该部分语句均采用ASCII字符，以及CR(carriage return)和LF(line feed)。每条语句以"$"开始，以<CR><LF>结束。其通用格式如下：

```
$ttsss,d1,d2,...,dn*hh<CR><LF>
```

tt 代表语句输出方的ID；
sss 代表语句ID；
d1、d2、dn 代表各个字段；
','  为每个字段的分隔符；

\*  为有效字段与语句校验和的分隔符；
hh代表语句的校验和，2个16进制字符。
校验和从tt开始到\*（不含\*）结束，每个字符依次按位异或。

## 输出方ID

| ID   | 说明                      |
| ---- | ------------------------- |
| GP   | 美国GPS导航系统定位       |
| GB   | 中国北斗导航系统定位      |
| GN   | 2个及以上导航系统联合定位 |

## 语句描述

| 序号 | 语句ID | 说明             |      |      |
| ---- | ------ | ---------------- | ---- | ---- |
| ---  | ---    | ---              |      |      |
| 1    | RMC    | 最简推荐导航信息 |      |      |
| 2    | VTG    | 航迹及对地速度   |      |      |
| 3    | GGA    | 定位信息         |      |      |
| 4    | GSV    | 可视卫星信息     |      |      |
| 5    | GSA    | 参与定位卫星信息 |      |      |
