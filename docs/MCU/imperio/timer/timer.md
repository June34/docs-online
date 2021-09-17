# TIMER设备



## 通用定时器简介

通用定时器是由一个通过可编程预分频器驱动的32位计数器构成。它适合多种用途，包含测量输入信号的脉冲宽度(输入捕获)，或者产生输出波形(PWM)。8288具有两组定时器，TIM0和TIM1功能完全相同且相互独立。

### 通用定时器主要特征

- 32位加计数器
- 3位可编程(可以实时修改)预分频器，定时器时钟=系统时钟/（预分频系数+1）
- 两种中断或事件类型可选择，即计数器溢出和比较结果相等触发



## 访问定时器设备

应用程序通过库函数提供的TIMER控制函数来访问TIMER设备硬件，相关接口如下所示：

| 函数                                                         | 描述                 |
| ------------------------------------------------------------ | -------------------- |
| `void timer_init(TIMER_TYPE *TIMERx, TIMER_CFG_Type *cfg);`  | 初始化定时器设备     |
| `void timer_enable(TIMER_TYPE *TIMERx);`                     | 使能定时器设备       |
| `void timer_disable(TIMER_TYPE *TIMERx);`                    | 关闭定时器设备       |
| `void timer_int_enable(TIMER_TYPE *TIMERx, TIMER_INT_TYPE it);` | 使能定时器中断       |
| `void timer_int_disable(TIMER_TYPE *TIMERx, TIMER_INT_TYPE it);` | 关闭定时器中断       |
| `void timer_int_clear_pending(TIMER_TYPE *TIMERx, TIMER_INT_TYPE it);` | 清除定时器中断标志位 |
| `void timer_set_count(TIMER_TYPE *TIMERx, uint32_t count);`  | 设置定时器计数值     |
| `uint32_t timer_get_count(TIMER_TYPE *TIMERx);`              | 获取定时器计数值     |
| `void timer_set_compare_value(TIMER_TYPE *TIMERx, uint32_t cmp);` | 设置定时器比较值     |



## 初始化定时器设备

通过下列函数对TIMER接口进行初始化设置:

```C
void timer_init(TIMER_TYPE *TIMERx, TIMER_CFG_Type *cfg)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));
    CHECK_PARAM(PARAM_TIMER_PRESCALER(cfg->pre));

    TIMERx->CTR |= (cfg->pre << 3);
    TIMERx->CMP = cfg->cmp;
    TIMERx->TRR = cfg->cnt;

}
```



## 使能定时器设备

通过下列函数对TIMER设备使能:

```C
void timer_enable(TIMER_TYPE *TIMERx)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));

	TIMERx->CTR |= (TIMER_ENABLE_MASK);

}
```



## 关闭定时器设备

通过下列函数关闭TIMER设备:

```C
void timer_disable(TIMER_TYPE *TIMERx)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));

	TIMERx->CTR &= ~(TIMER_ENABLE_MASK);

}
```



## 使能定时器中断

通过下列函数对TIMER设备的中断进行使能:

```C
void timer_int_enable(TIMER_TYPE *TIMERx, TIMER_INT_TYPE it)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));
	CHECK_PARAM(PARAM_TIMER_IT_TYPE(it));
	if (TIMERx == UC_TIMER0)
	{
		if (it == TIMER_IT_OVF)
		{
			IER |= (1<<28);
		}
		else
		{
			IER |= (1<<29);
		}
	}
	else
	{
		if (it == TIMER_IT_OVF)
		{
			IER |= (1<<30);
		}
		else
		{
			IER |= (1<<31);
		}
	}
}
```



## 关闭定时器中断

通过下列函数关闭TIMER设备的中断:

```C
void timer_int_disable(TIMER_TYPE *TIMERx, TIMER_INT_TYPE it)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));
	CHECK_PARAM(PARAM_TIMER_IT_TYPE(it));
	if (TIMERx == UC_TIMER0)
	{
		if (it == TIMER_IT_OVF)
		{
			IER &= ~(1<<28);
		}
		else
		{
			IER &= ~(1<<29);
		}
	}
	else
	{
		if (it == TIMER_IT_OVF)
		{
			IER &= ~(1<<30);
		}
		else
		{
			IER &= ~(1<<31);
		}
	}
}
```



## 清除定时器中断标志位

通过下列函数清除TIMER设备的中断标志位:

```C
void timer_int_clear_pending(TIMER_TYPE *TIMERx, TIMER_INT_TYPE it)
{
	CHECK_PARAM(PARAM_TIMER(TIMERx));
	CHECK_PARAM(PARAM_TIMER_IT_TYPE(it));
	if (TIMERx == UC_TIMER0)
	{
		if (it == TIMER_IT_OVF)
		{
			ICP |= (1<<28);
		}
		else
		{
			ICP |= (1<<29);
		}
	}
	else
	{
		if (it == TIMER_IT_OVF)
		{
			ICP |= (1<<30);
		}
		else
		{
			ICP |= (1<<31);
		}
	}
}
```



## 设置定时器计数值

通过下列函数设置TIMER设备的计数值:

```C
void timer_set_count(TIMER_TYPE *TIMERx, uint32_t count)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));

    TIMERx->TRR = count;

}
```



## 获取定时器计数值

通过下列函数获取TIMER设备的计数值:

```C
uint32_t timer_get_count(TIMER_TYPE *TIMERx)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));

    return TIMERx->TRR;

}
```



## 设置定时器比较值

通过下列函数设置TIMER设备的比较值:

```C
void timer_set_compare_value(TIMER_TYPE *TIMERx, uint32_t cmp)
{
    CHECK_PARAM(PARAM_TIMER(TIMERx));

    TIMERx->CMP = cmp;

}
```



## 定时器设备使用示例

```C
#include <stdio.h>
#include "int.h"
#include "timer.h"

void ISR_TA_OVF (void)
{
	uint32_t pre;
	timer_int_clear_pending(UC_TIMER0, TIMER_IT_OVF);

	pre = 7;
	timer_set_count(UC_TIMER0, 0xffffffffU - (SYSTEM_CLK/(pre+1))*1);//1S
	printf("timer0 overflow interrupt!!!\r\n");

}

void ISR_TB_CMP (void)
{
	timer_int_clear_pending(UC_TIMER1, TIMER_IT_CMP);

	printf("timer1 compare interrupt!!!\r\n");

}

void timer_ovf_demo(void)
{
	TIMER_CFG_Type cfg;
    printf("---------timer ovf demo start---------\r\n");

	cfg.pre = 7;
	cfg.cnt = 0xffffffffU - (SYSTEM_CLK/(cfg.pre+1))*1;//1S
	cfg.cmp = 0;//It must be 0 in this mode.
	timer_init(UC_TIMER0, &cfg);
	timer_enable(UC_TIMER0);
	timer_int_enable(UC_TIMER0, TIMER_IT_OVF);
	int_enable();
	
	printf("---------timer ovf demo end---------\r\n");

}

void timer_cmp_demo(void)
{
	TIMER_CFG_Type cfg;
    printf("---------timer cmp demo start---------\r\n");
	printf("You can measure the voltage of 'AUXDAC_OUPUT'!\r\n");

	cfg.pre = 5;
	cfg.cnt = 0;//clear counter
	cfg.cmp = (SYSTEM_CLK/(cfg.pre+1))*3;//3S
	timer_init(UC_TIMER1, &cfg);
	timer_enable(UC_TIMER1);
	timer_int_enable(UC_TIMER1, TIMER_IT_CMP);
	int_enable();
	
	printf("---------timer cmp demo end---------\r\n");

}

int main(int argc, char **argv)
{
    printf("-------------Timer Demo start--------------\n");

	timer_ovf_demo();
	timer_cmp_demo();
	
	printf("-------------Timer Demo end--------------\n");
	
	while(1);
	return 0;

}
```

