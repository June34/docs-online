# 直接存储器访问（uDMA）



## uDMA简介

uDMA控制器允许数据在后台传输，而不需要处理器的干预。如果使用得当，在进行大量数据传输的过程中能够显著的降低处理器的使用率。UC8288系列芯片总共有4个DMA通道。

**uDMA通道主要特征:**

- 4个DMA通道
- DMA传输位数为32位
- 全部通道支持FIFO缓冲，缓冲深度为4
- 支持SPI，ADC通过uDMA传输数据
- 多种传输模式，通过软件配置
- 存储器和存储器间的传输
- 外设和存储器、存储器和外设之间的传输

**uDMA功能与操作说明:**

- 开启DMA传输:

软件需要设置CCRn寄存器的27位去使能uDMA通道，然后再配置其他寄存器，例如传输数据尺寸，源地址，目的地址等，最后置位CCRn寄存器的28位开启传输。

- One-shot Data Moving：

为了实现One-shot Data Moving，在软件配置上需要设置模式位为00或10，如果是00，uDMA通道将会工作在normal mode 并且需要源内存地址和目的内存地址都是处于类似于RAM地址。

- Scatter-Gather Data Moving:

为了实现Scatter-Gather Data Moving，软件需要配置模式位为x1,在配置完成后uDMA 通道会从源总线加载linked buffer descriptor(linked-BD)，然后开始数据传输。

## 访问uDMA设备

| 函数                                 | 描述                   |
| ------------------------------------ | ---------------------- |
| `void udma_init();`                  | uDMA设备初始化         |
| `void udma_chan_init();`             | uDMA设备通道初始化     |
| `void udma_chan_set_source_addr();`  | uDMA设备源地址设置     |
| `void udma_chan_set_destina_addr();` | uDMA设备目的地址设置   |
| `void udma_chan_set_link_addr();`    | uDMA设备设置link地址   |
| `void UDMA_ClearFlag();`             | uDMA设备清除中断标志位 |
| `FunctionalState UDMA_GetITState();` | uDMA设备中断号获取     |



## uDMA设备初始化函数



```C
void udma_init(UDMA_TypeDef *DMA, DMA_CFG_Type *DMA_Config);
```



## uDMA设备通道初始化



```C
void udma_chan_init(UDCHAN_TypeDef *CHANx, DMACHAN_CFG_Type *CHAN_Config);
```



## uDMA设备源地址设置



```C
void udma_chan_set_source_addr(UDCHAN_TypeDef *CHANx, uint32_t s_addr);
```



## uDMA设备目的地址设置



```C
void udma_chan_set_destina_addr(UDCHAN_TypeDef *CHANx, uint32_t d_addr);
```



## uDMA设备设置link地址



```C
void udma_chan_set_link_addr(UDCHAN_TypeDef *CHANx, uint32_t l_addr);
```



## uDMA设备清除中断标志位



```C
void UDMA_ClearFlag(UDCHAN_TypeDef*CHANx);
```



## uDMA设备中断号获取



```C
FunctionalState UDMA_GetITState(uDMA_ITCH_Type UdmaChannel);
```



## uDMA应用示例



### Normal Mode：

```C
/*Include File*/
#include <stdio.h>
#include <pulpino.h>
#include <string_lib.h>
#include <utils.h>
#include "udma.h"
#include "int.h"


/*Definition*/

/*Static Variable*/

/*recode the number of interrupt */
volatile int udma_interrupt = 0;

static void Demo_LinkBDMode_Udma_Init(void);


/*interrupt service funciton*/
void ISR_UDMA(void)
{
	/*aquire interrupt flag*/
	if(UDMA_GetITState(UDMA_CHAN2_IT_TC)	==	ENABLE){
			/*clear interrupt flag*/
			UDMA_ClearFlag(UC_DMA_CHAN2);
			udma_interrupt++;
	}
	
}

void delay(int value)
{
    int i,j;
    for(i=0;i<value;i++)
		for(j=0;j<100;j++);
            
}


int main(int argc, char **argv)
{
	
	printf("2021-04-21\r\n");
	
/**************************************************************************************************
 * 
 * 
 * 											Normal Mode
 * 
 * 
 * ************************************************************************************************/
	/*source data*/
	uint32_t * udma_data_src;
	/*destination data*/
	uint32_t * udma_data_dst;
	
	/*Link source data*/
	uint32_t *data_test;
	
	uint16_t i,j;

	int a = 0x1f;
	int b = 0x5a;
	printf("a = %d, b = %d\r\n", a, b);
	
	/*enable interrupt*/
	int_enable();
	IER |= (1<<3);
	
	printf("Normal Mode Start !\r\n");
	/*set address*/
	udma_data_src = (uint32_t *)0x308000;
	
	/*set address*/
	udma_data_dst = (uint32_t *)0x309000;

	/*assignment*/
	for(i=0;i<256;i++)
        udma_data_src[i] = i;
		
	/*udma init*/	
	Demo_NormalMode_Udma_Init();
	
	/*wait block mode finish*/
	delay(100); 
	
	/*compare*/
    for(i=0;i<256;i++){
        if(udma_data_dst[i]!=i)
			printf("normal mode error i = %d\r\n",i);
	}
	printf("Normal Mode done !\r\n");
	
	printf("UDMA interrput count: %d\r\n", udma_interrupt);
    
    while(1);
	return 0;
}


static void Demo_NormalMode_Udma_Init(void){
		DMA_CFG_Type	dma_cfg;
		DMACHAN_CFG_Type	dma_chan_cfg;
		
		
		/*enable channel interrupt*/
		dma_cfg.channel_interrupt_mask 	= CHAN_INT_ENABLE;
		
		/*init udma*/
		udma_init(UC_DMA, &dma_cfg);
		
		/*channel reset*/
		dma_chan_cfg.channel_reset 		 = ENABLE;
		
		/*channel enable*/
		dma_chan_cfg.channel_enable 	 = ENABLE;
		
		/*set source address increase memory*/
		dma_chan_cfg.channel_s_increment = MEMORY;
		
		/*set distination address increase memory*/
		dma_chan_cfg.channel_d_increment = MEMORY;
		
		/*channel mode selece*/
		dma_chan_cfg.channel_mode 		 = NORMAL_MODE;
		
		/*channel run*/
		dma_chan_cfg.channel_pause 		 = DISABLE;
		
		/*channel open transfer*/
		dma_chan_cfg.channel_endof 		 = DISABLE;
		
		/*set data of transfer size*/
		dma_chan_cfg.channel_size 		 = 0x100;


		
		
		/*set source addr*/
		uint32_t s_addr = 0x308000;
		udma_chan_set_source_addr(UC_DMA_CHAN2, s_addr);
		
		/*set distination addr*/
		uint32_t d_addr = 0x309000;
		udma_chan_set_destina_addr(UC_DMA_CHAN2, d_addr);
		
		/*udma chammel initialize*/
		udma_chan_init(UC_DMA_CHAN2,&dma_chan_cfg);
	
	
}
```




### Link-BD Mode：

```C
/*Include File*/
#include <stdio.h>
#include <pulpino.h>
#include <string_lib.h>
#include <utils.h>
#include "udma.h"
#include "int.h"


/*Definition*/

/*Static Variable*/

/*recode the number of interrupt */
volatile int udma_interrupt = 0;

static void Demo_LinkBDMode_Udma_Init(void);


/*interrupt service funciton*/
void ISR_UDMA(void)
{
	/*aquire interrupt flag*/
	if(UDMA_GetITState(UDMA_CHAN2_IT_TC)	==	ENABLE){
			/*clear interrupt flag*/
			UDMA_ClearFlag(UC_DMA_CHAN2);
			udma_interrupt++;
	}
	
}

void delay(int value)
{
    int i,j;
    for(i=0;i<value;i++)
		for(j=0;j<100;j++);
            
}

int main(int argc, char **argv)
{
	
	printf("2021-04-21\r\n");
/**************************************************************************************************
 * 
 * 
 * 											LinkBD Mode
 * 
 * 
 * ************************************************************************************************/
 
	printf("LinkBD Mode Start !\r\n");
	/*set address*/
	data_test	=	(uint32_t *)0x303000;
	
	/*assignment*/
	for(i=0;i<256;i++)
        data_test[i] = i;
		
	/*udma init*/	
	Demo_LinkBDMode_Udma_Init();
	
	/*compare*/
	for(j=0;j<256;j++){
        if(data_test[j]!=j)
			printf("data test error\r\n"); // wait block mode finish 
	}
	
	/*set address*/
	udma_data_dst = (uint32_t *)0x303400;
	
	for(j=0;j<6;j++){
		for(i=0;i<256;i++){
			if(udma_data_dst[i]!=i){
				printf("link mode error i = %d\r\n",i);
				//printf("udma_data_dst = %d\r\n",udma_data_dst[i]);
			}
		}
			udma_data_dst	=	udma_data_dst	+	0x100;	
			printf("udma_data_dst = 0x%x\r\n",udma_data_dst);
			
	}
	
	printf("LinkBD Mode Done !\r\n");
	printf("UDMA interrput count: %d\r\n", udma_interrupt);
	while(1);
	return 0;
}

static void Demo_LinkBDMode_Udma_Init(void){
		DMA_CFG_Type		dma_cfg;
		DMACHAN_CFG_Type	dma_chan_cfg;
		
		UDCHAN_TypeDef*  bd_link_dma	=	0x302000;
		
		/*enable channel interrupt*/
		dma_cfg.channel_interrupt_mask 	= CHAN_INT_ENABLE;
		
		/*init udma*/
		udma_init(UC_DMA, &dma_cfg);
		
		/*channel reset*/
		dma_chan_cfg.channel_reset 		 = ENABLE;
		
		/*channel enable*/
		dma_chan_cfg.channel_enable 	 = ENABLE;
		
		/*set source address increase memory*/
		dma_chan_cfg.channel_s_increment = MEMORY;
		
		/*set distination address increase memory*/
		dma_chan_cfg.channel_d_increment = MEMORY;
		
		/*channel mode selece*/
		dma_chan_cfg.channel_mode 		 = LINK_BD_MODE;
		
		/*channel run*/
		dma_chan_cfg.channel_pause 		 = DISABLE;
		
		/*channel open transfer*/
		dma_chan_cfg.channel_endof 		 = DISABLE;
		
		/*set data of transfer size 0~4095*/
		dma_chan_cfg.channel_size 		 = 0x100;

		bd_link_dma[0].CHANSAR = 0x303000;
		bd_link_dma[0].CHANDAR = 0x303400;
		bd_link_dma[0].CHANNLD = &bd_link_dma[1];
		udma_chan_init(&bd_link_dma[0],&dma_chan_cfg);
		
		bd_link_dma[1].CHANSAR = 0x303000;
		bd_link_dma[1].CHANDAR = 0x303800;
		bd_link_dma[1].CHANNLD = &bd_link_dma[2];
		udma_chan_init(&bd_link_dma[1],&dma_chan_cfg);
		
		bd_link_dma[2].CHANSAR = 0x303000;
		bd_link_dma[2].CHANDAR = 0x303C00;
		bd_link_dma[2].CHANNLD = &bd_link_dma[3];
		udma_chan_init(&bd_link_dma[2],&dma_chan_cfg);
		
		bd_link_dma[3].CHANSAR = 0x303000;
		bd_link_dma[3].CHANDAR = 0x304000;
		bd_link_dma[3].CHANNLD = &bd_link_dma[4];
		udma_chan_init(&bd_link_dma[3],&dma_chan_cfg);
		
		bd_link_dma[4].CHANSAR = 0x303000;
		bd_link_dma[4].CHANDAR = 0x304400;
		bd_link_dma[4].CHANNLD = &bd_link_dma[5];
		udma_chan_init(&bd_link_dma[4],&dma_chan_cfg);
		
		bd_link_dma[5].CHANSAR = 0x303000;
		bd_link_dma[5].CHANDAR = 0x304800;
		bd_link_dma[5].CHANNLD = &bd_link_dma[6];
		udma_chan_init(&bd_link_dma[5],&dma_chan_cfg);
		
		bd_link_dma[6].CHANSAR = 0x303000;
		bd_link_dma[6].CHANDAR = 0x304C00;
		bd_link_dma[6].CHANNLD = &bd_link_dma[7];
		udma_chan_init(&bd_link_dma[6],&dma_chan_cfg);
		
		dma_chan_cfg.channel_endof = ENABLE;
		
		bd_link_dma[7].CHANSAR = 0x303000;
		bd_link_dma[7].CHANDAR = 0x305000;
		udma_chan_init(&bd_link_dma[7],&dma_chan_cfg);
		
		dma_chan_cfg.channel_endof = DISABLE;
		udma_chan_set_link_addr(UC_DMA_CHAN2, bd_link_dma);
		udma_chan_init(UC_DMA_CHAN2,&dma_chan_cfg);


}
```

