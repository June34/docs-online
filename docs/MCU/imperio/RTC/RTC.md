# RTC设备



## RTC简介

RTC （Real-Time Clock）实时时钟可以提供精确的实时时间，它可以用于产生年、月、日、时、分、秒等信息。UC8288采用内部32.768KHz时钟作为RTC时钟源。并且添加RTC报警功能，设置RTC报警日期和时间后在RTC设备计时到达后会产生一个RTC报警中断。





## 访问RTC设备

应用程序通过 RTC 设备管理接口来访问 RTC 硬件，相关接口如下所示：

| 函数                                                         | 描述                  |
| ------------------------------------------------------------ | --------------------- |
| `void rc32k_init(void);`                                     | 初始化32.768K时钟     |
| `void rtc_init(RTC_TypeDef *RTCx);`                          | RTC设备初始化         |
| `void rtc_enable(RTC_TypeDef *RTCx);`                        | 使能RTC设备           |
| `void rtc_disable(RTC_TypeDef *RTCx);`                       | 关闭RTC设备           |
| `void rtc_set_time(RTC_TypeDef *RTCx, rtc_time_t *rtc_time);` | 设置RTC日期和时间     |
| `void rtc_get_time(RTC_TypeDef *RTCx, rtc_time_t *rtc_time);` | 获取RTC日期和时间     |
| `void rtc_set_alarm(RTC_TypeDef *RTCx, rtc_alarm_t *rtc_alarm);` | 设置RTC报警日期和时间 |
| `void rtc_get_alarm(RTC_TypeDef *RTCx, rtc_alarm_t *rtc_alarm);` | 获取RTC报警日期和时间 |
| `void rtc_enable_alarm_interrupt(RTC_TypeDef *RTCx);`        | 使能RTC报警中断       |
| `void rtc_disable_alarm_interrupt(RTC_TypeDef *RTCx);`       | 关闭RTC报警中断       |
| `void rtc_clear_alarm_pending(RTC_TypeDef *RTCx);`           | 清除RTC报警中断标志   |



## 初始化RTC—32K时钟设备

通过如下程序初始化32.768K时钟设备：

```C
void rc32k_init(void)
{
#if 0
	rc32k_set_clock_freq(0x80);
	rc32k_set_bias_current(1);//set bias current code manually
	rc32k_calibrate();
#else
	//rc32k_autoset(RC32K_FREQ_FIRST);
	rc32k_autoset(RC32K_BIAS_FIRST);
#endif
    IER &= (~(1 << 0)); //close RTC alm interrput
    ICP |= 1; //clear RTC interrput pending
}
```



## 初始化RTC设备

通过如下函数初始化RTC设备：

```C
void rtc_init(RTC_TypeDef *RTCx)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));

    RTCx->CTRL &= ~(1U << 0);//enable rtc

    RTCx->TS0 = RTC_MAKE_HMS(0, 0, 0);//00:00:00
    RTCx->TS1 = RTC_MAKE_YMDW(RTC_YEAR_BASE, 1, 1, RTC_WDAY_SAT);//from 'RTC_YEAR_BASE'.01.01

    RTCx->CTRL = (1U << 1);//uprtc_time
    while ((RTCx->CTRL & (1U << 1)) != 0)//wait ready
    {
        asm("nop");
    }
}
```



## 使能RTC设备

通过如下函数使能RTC设备：

```C
void rtc_enable(RTC_TypeDef *RTCx)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));
    RTCx->CTRL &= ~(1U << 0);//enable rtc
}
```



## 失能RTC设备

通过如下函数关闭RTC设备：

```C
void rtc_disable(RTC_TypeDef *RTCx)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));
    RTCx->CTRL |= (1U << 0);//disable rtc
}

```



## 设置日期时间

通过如下函数设置 RTC 设备当前日期和时间：

```C
void rtc_set_time(RTC_TypeDef *RTCx, rtc_time_t *rtc_time)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));
    CHECK_PARAM(PARAM_RTC_Sec_RATE  (rtc_time->sec));
    CHECK_PARAM(PARAM_RTC_Min_RATE  (rtc_time->min));
    CHECK_PARAM(PARAM_RTC_Hour_RATE (rtc_time->hour));
    CHECK_PARAM(PARAM_RTC_Year_RATE ((rtc_time->year-RTC_YEAR_BASE)));
    CHECK_PARAM(PARAM_RTC_Mon_RATE  (rtc_time->mon));
    CHECK_PARAM(PARAM_RTC_Day_RATE  (rtc_time->day));

    RTCx->TS0 = RTC_MAKE_HMS(rtc_time->hour, rtc_time->min, rtc_time->sec);
    RTCx->TS1 = RTC_MAKE_YMDW(rtc_time->year, rtc_time->mon, rtc_time->day, rtc_time->week);

    RTCx->CTRL |= (1U << 1);//uprtc_time
    while ((RTCx->CTRL & (1U << 1)) != 0)//wait ready
    {
        asm("nop");
    }
}
```



## 获取当前时间

通过如下函数设置 RTC 获取当前时间值：

```C
void rtc_get_time(RTC_TypeDef *RTCx, rtc_time_t *rtc_time)
{
    uint32_t tm0,tm1;

    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));

    RTCx->CTRL |= (1U << 2);//
    while ((RTCx->CTRL & (1U << 2)) != 0)//wait ready
    {
        asm("nop");
    }

    tm0 = RTCx->TIM0;
    tm1 = RTCx->TIM1;
    rtc_time->year = ((tm1 >> 16) & 0x7f) + RTC_YEAR_BASE;
    rtc_time->mon = (tm1 >> 12) & 0x0f;
    rtc_time->day = (tm1 >> 4) & 0x1f;
    rtc_time->week = tm1 & 0x07;

    rtc_time->hour = (tm0 >> 16) & 0x1f;
    rtc_time->min = (tm0 >> 8) & 0x3f;
    rtc_time->sec = tm0 & 0x3f;
}
```



## 设置报警时间

通过如下函数设置 RTC 报警时间：

```C
void rtc_set_alarm(RTC_TypeDef *RTCx, rtc_alarm_t *rtc_alarm)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));
    CHECK_PARAM(PARAM_RTC_Sec_RATE  (rtc_alarm->sec));
    CHECK_PARAM(PARAM_RTC_Min_RATE  (rtc_alarm->min));
    CHECK_PARAM(PARAM_RTC_Hour_RATE (rtc_alarm->hour));
    CHECK_PARAM(PARAM_RTC_Year_RATE ((rtc_alarm->year-RTC_YEAR_BASE)));
    CHECK_PARAM(PARAM_RTC_Mon_RATE  (rtc_alarm->mon));
    CHECK_PARAM(PARAM_RTC_Day_RATE  (rtc_alarm->day));

    RTCx->AS0 = RTC_MAKE_HMS(rtc_alarm->hour, rtc_alarm->min, rtc_alarm->sec);
    RTCx->AS1 = RTC_MAKE_YMDW(rtc_alarm->year, rtc_alarm->mon, rtc_alarm->day, rtc_alarm->week);

    RTCx->ACTRL = (RTCx->ACTRL & 0x7f) | rtc_alarm->mask;//set rtc alarm mask
    RTCx->ACTRL |= (1U << 8);//enable rtc alarm
}
```



## 获取报警时间

通过如下函数获取 RTC 报警时间：

```C
void rtc_get_alarm(RTC_TypeDef *RTCx, rtc_alarm_t *rtc_alarm)
{
    uint32_t as0,as1;

    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));

    as0 = RTCx->AS0;
    as1 = RTCx->AS1;
    rtc_alarm->year = ((as1 >> 16) & 0x7f) + RTC_YEAR_BASE;
    rtc_alarm->mon = (as1 >> 12) & 0x0f;
    rtc_alarm->day = (as1 >> 4) & 0x1f;
    rtc_alarm->week = as1 & 0x07;

    rtc_alarm->hour = (as0 >> 16) & 0x1f;
    rtc_alarm->min = (as0 >> 8) & 0x3f;
    rtc_alarm->sec = as0 & 0x3f;

    rtc_alarm->mask = RTCx->ACTRL & 0x7f;
}
```

## 使能RTC报警中断

通过如下函数使能RTC报警中断：

```C
void rtc_enable_alarm_interrupt(RTC_TypeDef *RTCx)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));
	RTCx->ACTRL |= BIT(12);
    IER |= (1U << 0);
	RTCx->ACTRL |= BIT(9);
}
```



## 关闭RTC报警中断

通过如下函数关闭RTC报警中断：

```C
void rtc_disable_alarm_interrupt(RTC_TypeDef *RTCx)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));
    IER &= ~(1U << 0);
}
```



## 清除RTC报警中断标志

通过如下函数清除RTC报警中断标志：

```C
void rtc_clear_alarm_pending(RTC_TypeDef *RTCx)
{
    CHECK_PARAM(PARAM_RTC_ADDR(RTCx));
	RTCx->ACTRL |= 1<<12;//it will be done before clear pending
    ICP |= (1U << 0); //clear RTC interrput pending
}


```



## RTC设备使用示例

RTC 设备的具体使用方式可以参考如下示例代码:

```C
#include <stdio.h>
#include "int.h"
#include "event.h"
#include "rtc.h"

static void delay_ms(uint32_t nms)
{
    for(int i=0;i<nms;i++)
    {
        for(int j=0;j<9600;j++)
        {
            asm("nop");
        }
    }
}

void ISR_RTC(void)
{
   // rtc_disable_alarm_interrupt(UC_RTC);
	rtc_clear_alarm_pending(UC_RTC);
    printf("RTC INTERRUPT!\n");
}

int main(int argc, char **argv)
{
    rtc_time_t rtc_time;
    rtc_alarm_t rtc_alarm;
    printf("rtc demo begin\r\n");
    rc32k_init();
    rtc_init(UC_RTC);
    rtc_get_time(UC_RTC, &rtc_time);
    printf("init time: %d-%d-%d %d:%d:%d week=%d\r\n", rtc_time.year, rtc_time.mon, rtc_time.day, rtc_time.hour, rtc_time.min, rtc_time.sec, rtc_time.week);
    rtc_time.year = 2020;
    rtc_time.mon = 1;
    rtc_time.day = 1;
    rtc_time.week = RTC_WDAY_WED;
    rtc_time.hour = 12;
    rtc_time.min = 0;
    rtc_time.sec = 0;
    printf("set time: %d-%d-%d %d:%d:%d week=%d\r\n", rtc_time.year, rtc_time.mon, rtc_time.day, rtc_time.hour, rtc_time.min, rtc_time.sec, rtc_time.week);
    rtc_set_time(UC_RTC, &rtc_time);
    rtc_get_time(UC_RTC, &rtc_time);
    printf("get time: %d-%d-%d %d:%d:%d week=%d\r\n", rtc_time.year, rtc_time.mon, rtc_time.day, rtc_time.hour, rtc_time.min, rtc_time.sec, rtc_time.week);

    rtc_alarm.year = 2020;
    rtc_alarm.mon = 1;
    rtc_alarm.day = 1;
    rtc_alarm.week = RTC_WDAY_WED;
    rtc_alarm.hour = 12;
    rtc_alarm.min = 0;
    rtc_alarm.sec = 10;
    rtc_alarm.mask = RTC_AM_YEAR | RTC_AM_MON | RTC_AM_DAY | RTC_AM_WEEK;//repeat every day
    printf("set alarm: %d-%d-%d %d:%d:%d week=%d mask=%08x\r\n", rtc_alarm.year, rtc_alarm.mon, rtc_alarm.day, rtc_alarm.hour, rtc_alarm.min, rtc_alarm.sec, rtc_alarm.week, rtc_alarm.mask);
    rtc_set_alarm(UC_RTC, &rtc_alarm);
    rtc_get_alarm(UC_RTC, &rtc_alarm);
    printf("get alarm: %d-%d-%d %d:%d:%d week=%d mask=%08x\r\n", rtc_alarm.year, rtc_alarm.mon, rtc_alarm.day, rtc_alarm.hour, rtc_alarm.min, rtc_alarm.sec, rtc_alarm.week, rtc_alarm.mask);
    rtc_enable_alarm_interrupt(UC_RTC);
    int_enable(); //enable global interrupt
    while(1)
    {
        delay_ms(1000);
        rtc_get_time(UC_RTC, &rtc_time);
        printf("get time: %d-%d-%d %d:%d:%d week=%d\r\n", rtc_time.year, rtc_time.mon, rtc_time.day, rtc_time.hour, rtc_time.min, rtc_time.sec, rtc_time.week);
    }

	return 0;
}
```

