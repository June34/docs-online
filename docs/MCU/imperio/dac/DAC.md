# DAC综述 #
## DAC简介 ##
UC8088提供两个几乎一模一样的DAC，区别在于音频DAC（AUDIO_DAC，DAC默认表示AUDIO_DAC）的输入数据使用了2M Hz。音频DAC采样率最大支持2Msps(Million Samples per Second)，采样时钟由系统时钟分频产生，分频系数保存在寄存器中。常见的情况是系统时钟频率取131Mhz，分频系数取100（寄存器配置值99），此时DAC采样频率为1.31Msps）时钟进行了采样再进行数模转换，辅助DAC(AUX_DAC)使用时采样率很低，就没有采用固定时钟进行采样，而是对输入的数字信号直接进行数模转换。两个DAC的位宽都是10bit。输出电压范围均为0.1V~AVDD_CAP(模拟参考电压)-0.1V，电流驱动能力不超过1mA。
## 访问ADC设备 ##
访问ADC设备，库中提供了接口：

|函数|描述|
|:---:|:---:|
|dac_power_set()|初始化DAC器件|
|dac_fifo_clear()|清空DAC缓存|
|void dac_watermark_set()|设置DAC水线|
|is_dac_fifo_over_watermark()|查看dac缓存是否已达到采样水线|
|is_dac_fifo_full()|查看dac缓存是否已满|
|dac_write()|dac写数据|
|dac_clkdiv_set()|设置dac时钟分频|
|dac_int_enable()|dac中断使能|
|dac_int_disable()|dac中断失能|
|dac_int_clear_pending()|清dac中断标志位|

### 初始化DAC器件 ###
使能DAC器件，函数原型如下所示：  
```C

void dac_power_set(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 清空DAC缓存 ###
清空DAC缓存，函数原型如下所示：  
```C

void dac_fifo_clear(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 设置DAC水线 ###
设置DAC水线，函数原型如下所示：  
```C

void dac_watermark_set(ADDA_TypeDef *ADDA, uint8_t water_mark)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|water_mark|DAC输出水线|
|**返回值**|描述|
|无|-|

### 查询是否超出水线 ###
查询是否超出水线，函数原型如下所示：  
```C

bool is_dac_fifo_over_watermark(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|true|DAC缓存超过水线|
|false|DAC缓存未超过水线|

### 查询DAC缓存是否已满 ###
查询DAC缓存是否已满，函数原型如下所示：  
```C

bool is_dac_fifo_full(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|true|DAC缓存为空|
|false|DAC缓存非空|

### DAC写数据 ###
DAC写数据，函数原型如下所示：  
```C

void dac_write(ADDA_TypeDef *ADDA, uint16_t wdata)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|wdata|DAC输出的数据|
|**返回值**|描述|
|无|-|

### 设置DAC分频 ###
设置DAC分频，函数原型如下所示：  
```C

void dac_clkdiv_set(ADDA_TypeDef *ADDA, uint16_t clk_div)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|clk_div|DAC分频系数|
|**返回值**|描述|
|无|-|

### DAC中断使能 ###
DAC中断使能，函数原型如下所示：  
```C

void dac_int_enable(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### DAC中断失能 ###
DAC中断失能，函数原型如下所示：  
```C

void dac_int_disable(ADDA_TypeDef *ADDA)

```
|**参数**|**描述**|
|:---:|:---:|
|ADDA|操作句柄|
|**返回值**|描述|
|无|-|

### 清中断标志位 ###
清中断标志位，函数原型如下所示：  
```C

void dac_int_clear_pending(void)

```

## DAC使用示例 ##
以下DAC输出示例，生成正弦波电压。
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

static const uint16_t sine_tbl[] =
{
	512, 518, 524, 530, 536, 543, 549, 555, 561, 567, 573, 580, 586, 592, 598, 604,
	610, 616, 622, 628, 634, 640, 646, 652, 658, 664, 670, 676, 682, 688, 694, 699,
	705, 711, 717, 722, 728, 733, 739, 745, 750, 755, 761, 766, 772, 777, 782, 787,
	793, 798, 803, 808, 813, 818, 823, 828, 833, 837, 842, 847, 851, 856, 860, 865,
	869, 874, 878, 882, 886, 891, 895, 899, 903, 907, 910, 914, 918, 922, 925, 929,
	932, 936, 939, 942, 946, 949, 952, 955, 958, 961, 963, 966, 969, 972, 974, 977,
	979, 981, 984, 986, 988, 990, 992, 994, 996, 997, 999, 1001, 1002, 1004, 1005, 1007,
	1008, 1009, 1010, 1011, 1012, 1013, 1014, 1014, 1015, 1016, 1016, 1017, 1017, 1017, 1017, 1017,
	1018, 1017, 1017, 1017, 1017, 1017, 1016, 1016, 1015, 1014, 1014, 1013, 1012, 1011, 1010, 1009,
	1008, 1007, 1005, 1004, 1002, 1001, 999, 997, 996, 994, 992, 990, 988, 986, 984, 981,
	979, 977, 974, 972, 969, 966, 963, 961, 958, 955, 952, 949, 946, 942, 939, 936,
	932, 929, 925, 922, 918, 914, 910, 907, 903, 899, 895, 891, 886, 882, 878, 874,
	869, 865, 860, 856, 851, 847, 842, 837, 833, 828, 823, 818, 813, 808, 803, 798,
	793, 787, 782, 777, 772, 766, 761, 755, 750, 745, 739, 733, 728, 722, 717, 711,
	705, 699, 694, 688, 682, 676, 670, 664, 658, 652, 646, 640, 634, 628, 622, 616,
	610, 604, 598, 592, 586, 580, 573, 567, 561, 555, 549, 543, 536, 530, 524, 518,
	512, 505, 499, 493, 487, 480, 474, 468, 462, 456, 450, 443, 437, 431, 425, 419,
	413, 407, 401, 395, 389, 383, 377, 371, 365, 359, 353, 347, 341, 335, 329, 324,
	318, 312, 306, 301, 295, 290, 284, 278, 273, 268, 262, 257, 251, 246, 241, 236,
	230, 225, 220, 215, 210, 205, 200, 195, 190, 186, 181, 176, 172, 167, 163, 158,
	154, 149, 145, 141, 137, 132, 128, 124, 120, 116, 113, 109, 105, 101, 98, 94,
	91, 87, 84, 81, 77, 74, 71, 68, 65, 62, 60, 57, 54, 51, 49, 46,
	44, 42, 39, 37, 35, 33, 31, 29, 27, 26, 24, 22, 21, 19, 18, 16,
	15, 14, 13, 12, 11, 10, 9, 9, 8, 7, 7, 6, 6, 6, 6, 6,
	6, 6, 6, 6, 6, 6, 7, 7, 8, 9, 9, 10, 11, 12, 13, 14,
	15, 16, 18, 19, 21, 22, 24, 26, 27, 29, 31, 33, 35, 37, 39, 42,
	44, 46, 49, 51, 54, 57, 60, 62, 65, 68, 71, 74, 77, 81, 84, 87,
	91, 94, 98, 101, 105, 109, 113, 116, 120, 124, 128, 132, 137, 141, 145, 149,
	154, 158, 163, 167, 172, 176, 181, 186, 190, 195, 200, 205, 210, 215, 220, 225,
	230, 236, 241, 246, 251, 257, 262, 268, 273, 278, 284, 290, 295, 301, 306, 312,
	318, 324, 329, 335, 341, 347, 353, 359, 365, 371, 377, 383, 389, 395, 401, 407,
	413, 419, 425, 431, 437, 443, 450, 456, 462, 468, 474, 480, 487, 493, 499, 505
};

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

volatile static int send_ptr = 0;
void ISR_DAC(void)
{
	dac_int_clear_pending();

    while (!is_dac_fifo_full(UC_ADDA))
    {
        dac_write(UC_ADDA, sine_tbl[send_ptr]);
		send_ptr++;
		if(send_ptr >= sizeof(sine_tbl)/sizeof(sine_tbl[0]))
		{
			send_ptr = 0;
		}
    }
}

void dac_output_voltage(float vol)
{
	uint16_t val;

	val = vol/1.6*1024;//formula
	dac_power_set(UC_ADDA);
	dac_clkdiv_set(UC_ADDA, 60);
	dac_watermark_set(UC_ADDA, 128);

	dac_fifo_clear(UC_ADDA);
	for (int i = 0; i < 8; i++)
	{
		dac_write(UC_ADDA, val);
	}
	//auxdac_level_set(UC_ADDA, val);
    //delay 10ms
    for (int i = 0; i < 131*10*1000L; i++)
    {
        asm("nop");
    }
}

void dac_output_sine_wave(void)
{
	dac_power_set(UC_ADDA);
	dac_clkdiv_set(UC_ADDA, 131000000U/45000U);//45KHz@sys_clk=131MHz
	dac_watermark_set(UC_ADDA, 128);

	dac_fifo_clear(UC_ADDA);

	dac_int_enable(UC_ADDA);
	int_enable();
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
	printf("---------DAC demo start---------\r\n");

//	adc_dac_loopback_test_demo();
//	adc_battery_voltage_measure_demo();
	while(1)
	{
//		adc_temp_in_chip_measure_demo();
		adc_dac_loopback_test_demo();
	}
    return 0;
}
```





















