# 辅助ADC综述 #
## 简介 ##
UC8088的辅助ADC（AUX_ADC，ADC默认表示AUX_ADC）电路原理框图如下图所示。包含模拟路由器、缓冲器（Buffer）、单转双buffer（S2D）和逐次逼近型ADC（SQR-ADC）组成。  
![avatar](ADC.png)
SAR-ADC 位宽为12bit，最高采样率可以到360KSPS（360K、180K，90K，45K四个档位可选，可以通过寄存器来修改ADC的时钟频率），8088一共有4个通道连接到辅助ADC，第一路是温度传感器部分电路，第二、第三、第四通道分别为IN_A_CHANNEL、IN_B_CHANNEL，IN_C_CHANNEL为片外输入信号通道，与第二、三通道片外信号直接输入不同的是，第四通道片外信号进入片内后经过PGA放大后再送到辅助ADC进行量化，PGA增益范围为（0.915~30dB），可由寄存器进行控制。另外还有一路电池电压检测通路，该电压是由电池电压进行分压（默认值是除以2.8，该值可由寄存器进行选择）后送入到ADC通路。辅助ADC的输入电压范围为0.1V~AVDD_CAP-0.1，参考电压即为AVDD_CAP。
## 访问ADC设备 ##
访问ADC设备，库中提供了以下接口：

|函数|描述|
|:---:|:---:|
|adc_power_set()|初始化ADC设备|
|adc_reset()|ADC设备复位|
|adc_set_sample_rate()|设置采样率|
|adc_channel_select()|选择采样通道|
|adc_int_enable()|中断采样使能|
|adc_int_disable()|中断采样失能|
|adc_int_clear_pending()|清中断标志位|
|adc_wait_data_ready()|等待数据转换完成|
|adc_read()|读取ADC值|
|adc_watermark_set()|设置采样水线|
|is_adc_fifo_over_watermark()|查看adc缓存是否已达到采样水线|
|is_adc_fifo_empty()|查看adc缓存是否为空|
|adc_fifo_clear()|清空adc缓存|
|adc_vbat_measure_enable()|电源采样使能|
|adc_temp_source_sel()|设置温度传感器采样目标|
|adc_temp_sensor_enable()|温度传感器采用使能|

### 初始化ADC器件 ###
使能ADC器件，函数原型如下所示：  
```C

void adc_power_set(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 复位ADC器件 ###
复位ADC，函数原型如下所示：  
```C

void adc_reset(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 设置ADC采样率 ###
设置ADC采样率，函数原型如下所示：  
```C

void adc_set_sample_rate(ADDA_TypeDef *ADDA, ADC_SAMPLE_RATE sample_rate)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|sample_rate|ADC采样率|
|**返回值**|描述|
|无|-|

### 选择ADC采样通道 ###
选择ADC采样通道，函数原型如下所示：  
```C

void adc_channel_select(ADDA_TypeDef *ADDA, ADC_CHANNEL CHANNEL)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|CHANNEL|ADC输出通道|
|**返回值**|描述|
|无|-|

### 使能ADC中断 ###
使能ADC中断，函数原型如下所示：  
```C

void adc_int_enable(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 失能ADC中断 ###
失能ADC中断，函数原型如下所示：  
```C

void adc_int_disable(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 清ADC中断标志位 ###
清ADC中断标志位，函数原型如下所示：  
```C

void adc_int_clear_pending(void)

```

### 等待ADC采样结束 ###
等待ADC采样结束，函数原型如下所示：  
```C

void adc_wait_data_ready(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 读取ADC值 ###
读取ADC值，函数原型如下所示：  
```C

uint16_t adc_read(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|uint16_t|ADC采样值|

### 设置ADC水线 ###
设置ADC水线，函数原型如下所示：  
```C

void adc_watermark_set(ADDA_TypeDef *ADDA, uint8_t water_mark)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|water_mark|ASDC采样水线|
|**返回值**|描述|
|无|-|

### 查询是否超出水线 ###
查询是否超出水线，函数原型如下所示：  
```C

bool is_adc_fifo_over_watermark(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|true|采样值超出水线|
|false|采样值未超出水线|

### 查询ADC缓存是否为空 ###
查询ADC缓存是否为空，函数原型如下所示：  
```C

bool is_adc_fifo_empty(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|true|ADC缓存为空|
|false|ADC缓存非空|

### 清空ADC缓存 ###
清空ADC缓存，函数原型如下所示：  
```C

void adc_fifo_clear(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 使能电源ADC采样 ###
使能电源ADC采样，函数原型如下所示：  
```C

void adc_vbat_measure_enable(bool enable)

```
|**参数**|**描述**|
|:---:|:---:|
|enable|电源电压采样使能|
|**返回值**|描述|
|无|-|

### 设置温度传感器采样目标 ###
设置温度传感器采样目标，函数原型如下所示：  
```C

void adc_temp_source_sel(ADDA_TypeDef *ADDA, ADC_TEMP_SRC temp_src)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|temp_src|温度采样目标值|
|**返回值**|描述|
|无|-|

### 使能温度传感器采样 ###
设置温度传感器采样目标，函数原型如下所示：  
```C

void adc_temp_sensor_enable(ADDA_TypeDef *ADDA, bool enable)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|enable|温度传感器采样使能|
|**返回值**|描述|
|无|-|

## ADC使用示例 ##
以下提供采样电源电压、温度传感器等实例
```C
#include <stdio.h>
#include "adda.h"
#include "event.h"
#include "int.h"
#include "string_lib.h"


#define _DAC_OUT_SINE_WAVE
#define _ADC_SAMPLE_IN_ISR

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

volatile static int adc_sample_cnt = 0;
static uint16_t adc_buf[512];
void ISR_ADC(void)
{
	adc_int_clear_pending();
	while (!is_adc_fifo_empty(UC_ADDA))
	{
		if (adc_sample_cnt < sizeof(adc_buf)/sizeof(adc_buf[0]))
		{
			adc_buf[adc_sample_cnt++] = adc_read(UC_ADDA);
		}
		else
		{
			//adc_read(UC_ADDA);//discard
			adc_int_disable(UC_ADDA);
			break;
		}
	}
}

void adc_battery_voltage_measure_demo(void)
{
    uint32_t adc_val, adc_sum, adc_cnt;
	float bat_vol;
    printf("---------ADC battery voltage measure demo start---------\r\n");
    adc_power_set(UC_ADDA);
	adc_set_sample_rate(UC_ADDA, ADC_SR_45KSPS);
    adc_watermark_set(UC_ADDA, 128);

	adc_channel_select(UC_ADDA, ADC_CHANNEL_BAT);
	adc_vbat_measure_enable(true);

    adc_reset(UC_ADDA);
    adc_fifo_clear(UC_ADDA);

    adc_sum = 0;
    for (adc_cnt = 0; adc_cnt < 64; adc_cnt++)
    {
        adc_wait_data_ready(UC_ADDA);
        adc_sum += adc_read(UC_ADDA);
    }

    adc_val = ((adc_sum + (1<<5)))>>6;//div 64
	bat_vol = (float)adc_val/4096*1.6*2.8;//formula
    printf("adc_val = %d bat_vol = %.2oV\r\n", adc_val, *((int *)&bat_vol));
	printf("---------ADC battery voltage measure demo end---------\r\n");
}

void adc_temp_in_chip_measure_demo(void)
{
    uint32_t adc_val, adc_sum, adc_cnt;
	float temp_val;
	int adc_data = 0;
	
    printf("---------ADC temp in-chip measure demo start---------\r\n");
											
	temp_in_pt1000(UC_ADDA);
    adc_reset(UC_ADDA);
	delay_ms(500);
	
    adc_fifo_clear(UC_ADDA);

    adc_sum = 0;
    for (adc_cnt = 0; adc_cnt < 64; adc_cnt++)
    {
        adc_wait_data_ready(UC_ADDA);
        adc_sum += adc_read(UC_ADDA);
    }

    adc_val = ((adc_sum + (1<<5)))>>6;//div 64
	temp_val = (float)(adc_val-1844)/7.3;//formula 2521
    printf("adc_val = %d temp_vol = %.1o Celsius\r\n", adc_val, *((int *)&temp_val));
	printf("---------ADC temp in-chip measure demo end---------\r\n");
}

void adc_dac_loopback_test_demo(void)
{
    uint32_t adc_cnt;
    printf("---------ADC & DAC loopback test demo start---------\r\n");
	printf("Before testing, you should connect 'AUDIO' to 'ADC_A'!\r\n");
#ifdef _DAC_OUT_SINE_WAVE
	dac_output_sine_wave();//varibale voltage
#else
	dac_output_voltage(0.8);//stable voltage
#endif

    adc_power_set(UC_ADDA);
	adc_set_sample_rate(UC_ADDA, ADC_SR_45KSPS);
    adc_watermark_set(UC_ADDA, 128);

	adc_channel_select(UC_ADDA, ADC_CHANNEL_C);

    adc_reset(UC_ADDA);
    adc_fifo_clear(UC_ADDA);

#ifdef _ADC_SAMPLE_IN_ISR
	adc_int_enable(UC_ADDA);
	int_enable();
	while (adc_sample_cnt < sizeof(adc_buf)/sizeof(adc_buf[0]))
	{
		asm("nop");
	}
#else
    for (adc_cnt = 0; adc_cnt < sizeof(adc_buf)/sizeof(adc_buf[0]); adc_cnt++)
    {
        adc_wait_data_ready(UC_ADDA);
        adc_buf[adc_cnt] = adc_read(UC_ADDA);
    }
#endif
    for (adc_cnt = 0; adc_cnt < sizeof(adc_buf)/sizeof(adc_buf[0]); adc_cnt++)
    {
		printf("adc_buf[%d] = %d\r\n", adc_cnt, adc_buf[adc_cnt]);
    }

	printf("---------ADC & DAC loopback test demo end---------\r\n");
}

int main(int argc, char **argv)
{
	printf("---------ADC demo start---------\r\n");

	while(1)
	{
		adc_battery_voltage_measure_demo();
		adc_temp_in_chip_measure_demo();
		adc_dac_loopback_test_demo();
	}
    return 0;
}
```

