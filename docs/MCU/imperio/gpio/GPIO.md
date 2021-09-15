# 通用I/O（GPIO）综述 #
## 简介 ##
GPIO控制器可以驱动或者获取信号/数据。通过相应的寄存器，可以定义I/O的功能、方向、状态以及中断。
### 主要特征 ###
&emsp;&emsp;-&emsp;受控I/O个数微29  
&emsp;&emsp;-&emsp;I/O输入/输出可配  
&emsp;&emsp;-&emsp;输入/输出状态：上拉、浮空  
&emsp;&emsp;-&emsp;所有I/O均可作为中断源，触发方式可配：低电平、高电平、上升沿、下降沿  
&emsp;&emsp;-&emsp;I/O功能多重复用  
## 操作GPIO ##
操作GPIO由SDK提供接口，相关接口如下：  

set

|函数|描述|
|:---:|:---:|
|gpio_init()|初始化GPIO|
|gpio_set_pin_mux()|设置IO复用功能|
|gpio_get_pin_mux()|获取IO复用功能|
|gpio_set_pin_pupd()|设置IO上下拉|
|gpio_get_pin_pupd()|获取IO的上下拉|
|gpio_set_pin_direction()|设置IO状态|
|gpio_get_pin_direction()|获取IO的状态|
|gpio_set_pin_value()|设置IO的输出状态|
|gpio_get_pin_value()|获取IO的输入状态|
|gpio_set_irq_en()|使能IO中断|
|gpio_set_irq_type()|设置IO中断触发方式|
|gpio_get_irq_status()|获取中断状态|
|gpio_int_enable()|使能GPIO中断|
|gpio_int_clear_pending()|清中断标志位|
|gpio_int_disable()|失能GPIO中断|

### 初始化GPIO ###
使用IO设备前，需对使用的IO口就行初始化操作，初始化接口函数原型如下：  
`void gpio_init(GPIO_TypeDef *GPIO,GPIO_CFG_TypeDef *GPIO_CFG,GPIO_CFG_Type *gpio_cfg)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO|IO操作句柄|
|GPIO_CFG|设备信息|
|gpio_cfg|配置信息|
|**输出参数**|-|
|GPIO|IO操作句柄|
|GPIO_CFG|设备信息|
|**返回值**|-|
|无|无|

### 设置GPIO复用功能 ###
该接口用于设置IO除主功能外的其他功能，需在初始化完成之后调用，若只使用IO的主功能则不需调用此函数，接口原型如下：    
`void gpio_set_pin_mux(GPIO_CFG_TypeDef *GPIO_CFG, GPIO_PIN pin, GPIO_FUNCTION func)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO_CFG|设备信息|
|pin|IO操作句柄|
|func|复用功能|
|**输出参数**|-|
|GPIO_CFG|设备信息|
|**返回值**|-|
|无|无|

### 获取GPIO复用功能 ###
该接口需在初始化完成之后调用，用于获取IO的复用功能，也可不调用，接口原型如下：    
`GPIO_FUNCTION gpio_get_pin_mux(GPIO_CFG_TypeDef *GPIO_CFG, GPIO_PIN pin)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO_CFG|设备信息|
|pin|IO操作句柄|
|**输出参数**|-|
|GPIO_CFG|设备信息|
|**返回值**|-|
|引脚功能|无|

### 设置GPIO上下拉 ###
IO口的上下拉需根据外围电路的需求进行配置，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`void gpio_set_pin_pupd(GPIO_CFG_TypeDef *GPIO_CFG, GPIO_PIN pin, GPIO_PUPD pupd)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO_CFG|设备信息|
|pin|IO操作句柄|
|pupd|指定的上下拉|
|**输出参数**|-|
|GPIO_CFG|设备信息|
|**返回值**|-|
|无|无|

### 获取GPIO上下拉配置 ###
IO口的上下拉需根据外围电路的需求进行配置，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`GPIO_PUPD gpio_get_pin_pupd(GPIO_CFG_TypeDef *GPIO_CFG, GPIO_PIN pin)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO_CFG|设备信息|
|pin|IO操作句柄|
|**输出参数**|-|
|GPIO_CFG|设备信息|
|**返回值**|-|
|IO口上下拉配置|无|

### 设置GPIO输入输出 ###
使用GPIO应对其输入输出进行设置，若不设置，则默认输出，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`void gpio_set_pin_direction(GPIO_TypeDef *GPIO, GPIO_PIN pin, GPIO_DIRECTION dir)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO|IO句柄|
|pin|IO操作句柄|
|dir|IO输入输出方向|
|**输出参数**|-|
|GPIO|IO句柄|
|**返回值**|-|
|无|无|

### 获取GPIO输入输出状态 ###
获取GPIO输入输出配置，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`GPIO_DIRECTION gpio_get_pin_direction(GPIO_TypeDef *GPIO, GPIO_PIN pin)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO|IO句柄|
|pin|IO操作句柄|
|**输出参数**|-|
|GPIO|IO句柄|
|**返回值**|-|
|IO输入输出状态|无|

### 设置GPIO输出状态 ###
GPIO可被设置为高低两种状态，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`void gpio_set_pin_value(GPIO_TypeDef *GPIO, GPIO_PIN pin, GPIO_VALUE value)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO|IO句柄|
|pin|IO操作句柄|
|value|输出的高低状态|
|**输出参数**|-|
|GPIO|IO句柄|
|**返回值**|-|
|无|无|

### 获取GPIO输入状态 ###
获取GPIO的输入状态，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`GPIO_VALUE gpio_get_pin_value(GPIO_TypeDef *GPIO, GPIO_PIN pin)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO|IO句柄|
|pin|IO操作句柄|
|**输出参数**|-|
|GPIO|IO句柄|
|**返回值**|-|
|IO状态|无|

### 使能GPIO中断 ###
使能GPIO中断，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`void gpio_set_irq_en(GPIO_TypeDef *GPIO, GPIO_PIN pin, uint8_t enable)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO|IO句柄|
|pin|IO操作句柄|
|enable|使能指示|
|**输出参数**|-|
|GPIO|IO句柄|
|**返回值**|-|
|无|无|

使能IO中断还可用一下接口：  
`void gpio_int_enable(void)；` 

### 获取GPIO中断状态 ###
获取GPIO中断状态，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`uint32_t gpio_get_irq_status(GPIO_TypeDef *GPIO)；`  

|**输入参数**|**描述**|
|:---:|:---:|
|GPIO|IO句柄|
|**输出参数**|-|
|GPIO|IO句柄|
|**返回值**|-|
|中断状态|无|

### 清GPIO中断标志 ###
清GPIO中断标志位，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`void gpio_int_clear_pending(void)；`  

### 使能GPIO中断 ###
清GPIO中断标志位，调用该接口应在被操作的IO口初始化之后，接口原型如下：  
`void gpio_int_disable(void)；`  

## 操作GPIO实例 ##
GPIO的使用方法可参考如下示例代码，操作步骤：  
&emsp;&emsp;1.初始化IO口；  
&emsp;&emsp;2.设置IO复用功能（可选）；  
&emsp;&emsp;3.设置IO输入输出功能（可选）；  
&emsp;&emsp;4.配置IO上下拉（可选）；  
&emsp;&emsp;5.设置IO输入输出状态（可选）；  
```C
#include <stdio.h>
#include "int.h"
#include "gpio.h"
#include "event.h"

static void delay_ms(uint32_t nms)
{
    for(int i=0;i<nms;i++)
    {
        for(int j=0;j<4;j++) //for(int j=0;j<4500*3;j++)
        {
            asm("nop");
        }
    }
}

/*
 * @brief   GPIO interrupt service function
 * @param 	None
 * @retval  None
 */
void ISR_GPIO(void)
{
    uint32_t pin, status;

    status = gpio_get_irq_status(UC_GPIO);//get gpip irq status
    gpio_int_clear_pending();//clear gpio interrupt pending

    for (pin=GPIO_PIN_0; pin<=GPIO_PIN_29; pin++)
    {
        if (status & (1<<pin))
        {
            printf("GPIO %d INTERRPUT\r\n", pin);
        }
    }
}

int main(int argc, char **argv)
{
    GPIO_DIRECTION dir;
    printf("-------------GPIO Example--------------\r\n");
	printf("Before testing, you should connect 'GPIO_3' to 'GPIO_4'!\r\n");
    printf("-------------start--------------\r\n");

	gpio_set_pin_value(UC_GPIO, GPIO_PIN_11, GPIO_VALUE_HIGH);
	gpio_set_pin_value(UC_GPIO, GPIO_PIN_26, GPIO_VALUE_HIGH);
	printf("-------------end--------------\r\n");
    while(1);
	return 0;
}
```
