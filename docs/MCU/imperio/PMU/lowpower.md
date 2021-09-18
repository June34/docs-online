# LOWPOWER #
## 低功耗简介 ##
UC8088的数字低功耗设计框架如下图所示，超低功耗部分由一个低功耗 LDO，即 LDO_LP 供电， 该 LDO 输出 1.0V， 只为电源管理模块（PMU）以及 RTC 部分的数字逻辑供电。UC8088 内嵌 DCDC 转换器电路， 能将外部供电降至 2.5V。然后大功率 LDO 再次将此电压降低到数字部 分所需要的核心电压。  
![avatar](sys_power.png)
在低功耗休眠模式下， LDO_LP 提供不超过 10uA 的电流。1.0V 电压使得 RTC 所消耗的 功率进一步降低。RTC 内部对 32KHz 的时钟采用链条降频电路， 降低电路的反转频率，进 一步降低功耗。
## 访问PMU ##
访问电源管理模块，库中提供了以下接口：

|函数|描述|
|:---:|:---:|
|pmu_enter_idle()|CPU进入挂起状态|
|pmu_enter_sleep(ADDA_TypeDef *ADDA)|CPU进入低功耗睡眠状态|
|pmu_enter_deepsleep()|CPU进入低功耗深度睡眠状态|

### CPU进入IDLE状态 ###
该接口使CPU进入挂起状态，当CPU被gated或被事件触发时，程序从原处开始运行，函数原型如下所示：  
```C

void pmu_enter_idle(PMU_TYPE *PMU);

```
|**参数**|**描述**|
|:---:|:---:|
|PMU|操作句柄|
|**返回值**|描述|
|无|-|

### CPU进入睡眠 ###
使用该接口将回使CPU进入睡眠状态，当事件或中断触发时，CPU被唤醒，程序从原处开始运行，函数原型如下所示：  
```C

void pmu_enter_sleep(PMU_TYPE *PMU, EX_WAKEUP_MASK_t method);

```
|**参数**|**描述**|
|:---:|:---:|
|PMU|操作句柄|
|method|外部唤醒使能|
|**返回值**|描述|
|无|-|

### CPU进入深度睡眠 ###
CPU进入深度睡眠模式，当CPU被事件或中断触发时，CPU被唤醒并被软件复位，程序从地址为0处开始运行，函数原型如下所示：  
```C

void pmu_enter_deepsleep(PMU_TYPE *PMU, EX_WAKEUP_MASK_t method);

```
|**参数**|**描述**|
|:---:|:---:|
|PMU|操作句柄|
|method|外部唤醒使能|
|**返回值**|描述|
|无|-|

## PMU使用示例 ##
以下提供3种CPU睡眠方式的实例。
```C
#include <stdio.h>
#include "pmu.h"
#include "gpio.h"

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
	GPIO_CFG_Type gpio_cfg;
	
	delay_ms(1000);			//in case cpu lucked
	while(1)
	{
		//LED show before sleep
		gpio_set_pin_value(UC_GPIO, GPIO_PIN_0, GPIO_VALUE_LOW);
		gpio_set_pin_value(UC_GPIO, GPIO_PIN_1, GPIO_VALUE_HIGH);
		delay_ms(5*1000);
		
		printf("cpu enter sleep.\r\n");
		delay_ms(50);						/* 等待打印完成 */
		
		pmu_enter_idle(UC_PMU);
//		pmu_enter_sleep(UC_PMU, EX_WAKEUP_EN);
//		pmu_enter_deepsleep(UC_PMU, EX_WAKEUP_EN);
		
		//LED show after wake up
		gpio_set_pin_value(UC_GPIO, GPIO_PIN_0, GPIO_VALUE_HIGH);
		gpio_set_pin_value(UC_GPIO, GPIO_PIN_1, GPIO_VALUE_LOW);
		delay_ms(5*1000);
	}
	return 0;
}
```

