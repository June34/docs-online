# 概述

gprs_customer下根据需求创建了不同的工程，以下说明了每个工程的应用方向、工程代码的目录结构以及代码的入口。

# 工程列表

| 工程名              | 调用库名               | 描述                              |
| ------------------- | ---------------------- | --------------------------------- |
| OS_gprs             | lib_gprs.a             | 支持GPRS、IP协议栈                |
| OS_gprs_eat         | lib_gprs_eat.a         | 支持GPRS、IP协议栈、EAT           |
| OS_gprs_gps         | lib_gprs_gps.a         | 支持GPRS、GPS、IP协议栈           |
| OS_gprs_gps_eat     | lib_gprs_gps_eat.a     | 支持GPRS、GPS、IP协议栈、EAT      |
| OS_gprs_iot_gps     | lib_gprs_iot_gps.a     | 支持GPRS、GPS、IP协议栈、IOT      |
| OS_gprs_iot_gps_eat | lib_gprs_iot_gps_eat.a | 支持GPRS、GPS、IP协议栈、IOT、EAT |

# 代码目录结构

关于代码树中各个目录存放的源代码的相关内容简介如下：

| 一级目录 | 二级目录 | 描述                     |
| -------- | -------- | ------------------------ |
| arch     | FPGA     | BOOT                     |
| driver   | ucchip   | 驱动代码                 |
|          | cce      | CCE                      |
| OS       | freertos | 系统代码                 |
|          | system   | 版本信息                 |
| platform | include  | 平台公共头文件           |
| PS       | app      | 包含EAT、GPS等应用头文件 |
|          | gprs     | GPRS应用头文件           |
| solution | OS       | 解决方案代码             |

# 代码入口

gprs_customer 入口在工程对应的 /solution/OS/main.c 中，基本流程如下：

```c
__reset1 int main(void)
{
    static_inform_init();//静态数据的初始化。前面不允许增加代码。
    
    #ifdef _ON_WATCH_
    __SET_WD_FEED_TIME_MS__(WATCHDOG_TIME);//设置看门狗的喂狗时间
    __ENABLE_WD__();//启动看门狗
    #endif
    
    rtc_init(); //RTC初始化
    lpm_init(); //LPM初始化
    vTraceEnable(TRC_START); //初始化trace工具
        
    __FEED_WD__();
    InitFreeRTOSIP();//TCP/IP初始化
    
     __FEED_WD__();
    app_startup();//初始化gprs协议栈
    
     __FEED_WD__();

    vTaskStartScheduler();//FreeRTOS任务调度开启
}
```

