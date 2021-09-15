# PWM 设备



## PWM简介

PWM(Pulse Width Modulation , 脉冲宽度调制) 是一种对模拟信号电平进行数字编码的方法，通过不同频率的脉冲使用方波的占空比用来对一个具体模拟信号的电平进行编码，使输出端得到一系列幅值相等的脉冲，用这些脉冲来代替所需要波形的设备。![pwm-f](pwm-f.png)

上图是一个简单的 PWM 原理示意图，假定定时器工作模式为向上计数，当计数值小于阈值时，则输出一种电平状态，比如高电平，当计数值大于阈值时则输出相反的电平状态，比如低电平。当计数值达到最大值是，计数器从0开始重新计数，又回到最初的电平状态。高电平持续时间（脉冲宽度）和周期时间的比值就是占空比，范围为0~100%。上图高电平的持续时间刚好是周期时间的一半，所以占空比为50%。



## 访问PWM设备

应用程序通过库函数提供的PWM控制函数来访问PWM设备硬件，相关接口如下所示：

| 函数                                                     | 描述          |
| -------------------------------------------------------- | ------------- |
| `void pwm_enable(PWM_TypeDef* PWM);`                     | 使能PWM       |
| `void pwm_disable(PWM_TypeDef* PWM);`                    | 关闭PWM       |
| `void pwm_set_period(PWM_TypeDef* PWM, int period_cnt);` | 设置PWM周期   |
| `void pwm_set_duty(PWM_TypeDef* PWM, int duty_cnt);`     | 设置PWM占空比 |



## 设置PWM周期

通过下列函数对PWM的周期进行设置:

```C
void pwm_set_period(PWM_TypeDef* PWM, int period_cnt)
{
    CHECK_PARAM(PARAM_PWM(PWM));

    PWM->CNTMAX = period_cnt;

}
```



## 设置PWM占空比

通过下列函数设置PWM的占空比：

```C
void pwm_set_duty(PWM_TypeDef* PWM, int duty_cnt)
{
    CHECK_PARAM(PARAM_PWM(PWM));

    int max_cnt = PWM->CNTMAX;
    if(duty_cnt > max_cnt)
        PWM->DUTY = max_cnt;
    else
        PWM->DUTY = duty_cnt;

}
```



## 使能PWM设备

通过下列函数使能PWM设备:

```C
void pwm_enable(PWM_TypeDef* PWM)
{
    CHECK_PARAM(PARAM_PWM(PWM));

    PWM->CTRL |= (1<<0);

}
```



## 关闭PWM设备

通过下列函数关闭PWM设备：

```C
void pwm_disable(PWM_TypeDef* PWM)
{
    CHECK_PARAM(PARAM_PWM(PWM));

    PWM->CTRL &= ~(1<<0);

}
```



## PWM设备使用示例

```C
#include <stdio.h>
#include "gpio.h"
#include "pwm.h"

int main(int argc, char **argv)
{
	gpio_set_pin_mux(UC_GPIO_CFG, GPIO_PIN_15, GPIO_FUNC_2);//PWM_OUT0

    pwm_enable(UC_PWM0);
    
    pwm_set_period(UC_PWM0, 1000);
    
    pwm_set_duty(UC_PWM0, 300);
    
    while(1);
    return 0;

}
```

8288PWM脉冲宽度调制器使用系统注释中为计数器时钟源

