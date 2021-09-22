# 真随机数 #
## 真随机数简介 ##
在UC芯片中，真随机数由硬件产生，操作时直接读取寄存器内的数据即可，无需其他操作。
## 访问TRNG ##
**访问地址：0x1A107080**

## 真随机数使用示例 ##
真随机数使用实例。
```C
#include <stdio.h>

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
	/* 真随机数无需驱动，直接读取寄存器的值 */
	int random_num = 0;
	uint32_t *ptr = (uint32_t *)(0x1A107080);
	
	printf("Systerm power on.\r\n");
	delay_ms(1000);			//in case cpu lucked
	
	while(1)
	{
		random_num = *ptr;
		printf("random number is: %d\r\n", random_num);
		delay_ms(5*1000);
	}
	return 0;
}
```

