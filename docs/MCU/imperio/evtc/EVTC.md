# EVTC #
## 事件控制器（EVTC）简介 ##
EVTC是事件控制器模块，用于事件源状态挂起、使能事件源，屏蔽事件源，及主动提起事件源。事件触发后，可用于退出CPU-GATED低功耗模式。EVTC主要特征：  
&emsp;&emsp;-&emsp;最多支持32个事件源；  
&emsp;&emsp;-&emsp;支持事件源的主动提起和屏蔽；  
&emsp;&emsp;-&emsp;支持事件源状态挂起；  
## 访问事件控制器 ##
，库中提供了以下接口：

|函数|描述|
|:---:|:---:|
|evtc_enable()|事件源使能|
|evtc_disable()|事件源失能|
|evtc_set_pending()|事件源状态挂起|
|evtc_set_unpend()|取消事件源挂起状态|
|evtc_get_status()|获取事件源挂起状态|
|evtc_trig_event()|事件源提起|
|evtc_shield_masks()|事件源屏蔽|
|evtc_unshield_masks()|事件源禁屏蔽|

### 事件源使能 ###
事件源使能，调用该接口，对应的事件源被允许在事件源状态挂起寄存器中挂起，从而触发CPU-GATED模式，函数原型如下所示：  
```C

void evtc_enable(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|无|-|

### 事件源失能 ###
事件源失能，调用该接口，对应的事件源将被禁止触发，函数原型如下所示：  
```C

void evtc_disable(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|无|-|

### 事件源状态挂起 ###
事件源状态挂起，调用该接口，当有事件源触发时，对应的事件将被挂起，并退出CPU-GATED模式，函数原型如下所示：  
```C

void evtc_set_pending(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|无|-|

### 取消事件源挂起状态 ###
取消事件源挂起状态，调用该接口，当有事件源触发时，对应的事件不会被挂起，函数原型如下所示：  
```C

void evtc_set_unpend(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|无|-|

### 获取事件源挂起状态 ###
获取事件源挂起状态，调用该接口可获取事件源挂起状态，函数原型如下所示：  
```C

uint8_t evtc_get_status(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|uint8_t|事件源挂起状态|

### 事件源提起 ###
事件源提起，调用该接口，对应事件源被主动提起，函数原型如下所示：  
```C

void evtc_trig_event(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|无|-|

### 事件源屏蔽 ###
事件源屏蔽，调用该接口，对应事件源被屏蔽，函数原型如下所示：  
```C

void evtc_shield_masks(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|无|-|

### 事件源禁屏蔽 ###
事件源禁屏蔽，调用该接口，对应事件源不会被屏蔽，函数原型如下所示：  
```C

void evtc_unshield_masks(EVTC_TYPE *EVTC, EVTC_TYPE_t evtc_type);

```
|**参数**|**描述**|
|:---:|:---:|
|EVTC|操作句柄|
|evtc_type|目标事件源|
|**返回值**|描述|
|无|-|

## EVTC使用示例 ##
以下提供使用UART作为事件源触发事件实例。
```C
#include <stdio.h>
#include "evtc.h"
#include "pmu.h"

static void delay_ms(uint32_t nms)
{
    for(int i=0;i<nms;i++)
    {
        for(int j=0;j<4500*3;j++)
        {
            asm("nop");
        }
    }
}

int main(int argc, char **argv)
{
	printf("Systerm power on.\r\n");
	delay_ms(1000);			//in case cpu lucked
	
	printf("cpu enter idle mode");
	evtc_enable(UC_EVTC, UART0_EVTC);
	pmu_enter_idle(UC_PMU);
	printf("cpu gated");
	
	while(1);
	return 0;
}
```

