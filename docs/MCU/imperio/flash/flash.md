# FLASH #
## FLASH简介 ##
UC芯片使用的FLASH是SPI FLASH，读写需通过SPI操作。

## 访问内部FLASH ##
访问内部FLASH，库中提供了以下接口：

|函数|描述|
|:---:|:---:|
|ReadFlashID()|获取FLASH ID|
|Flash_Read_SR()|获取FLASH操作状态|
|FlashEraseSector()|擦除地址所在扇区FLASH|
|FlashWrite()|FLASH写数据|
|FlashRead()|FLASH读数据|
|FlashPageProgram()|FLASH写数据|
|FlashEnableWr()|使能FLASH写数据|

### 获取FLASH ID ###
读取FLASH ID，函数原型如下所示：  
```C

uint32_t ReadFlashID(void);

```
|**参数**|**描述**|
|:---:|:---:|
|无|无|
|**返回值**|描述|
|uint32_t|FLASH ID|

### 获取FLASH状态 ###
获取FLASH状态，函数原型如下所示：  
```C

uint32_t Flash_Read_SR();

```
|**参数**|**描述**|
|:---:|:---:|
|无|无|
|**返回值**|描述|
|uint32_t|FLASH状态|

### 擦除扇区 ###
擦除FLASH需以扇区为单位进行擦除，函数原型如下所示：  
```C

void FlashEraseSector( uint32_t nBaseAddr);

```
|**参数**|**描述**|
|:---:|:---:|
|nBaseAddr|擦除扇区地址|
|**返回值**|描述|
|无|-|

### FLASH写入数据 ###
FLASH写数据，函数原型如下所示：  
```C

void FlashWrite( uint32_t nAddr,  const uint8_t *pData,  uint16_t usLen);

```
|**参数**|**描述**|
|:---:|:---:|
|nAddr|操作地址|
|pData|写入数据的指针|
|usLen|数据长度|
|**返回值**|描述|
|无|-|

### FALSH读出数据 ###
FLASH读出数据，函数原型如下所示：  
```C

void FlashQRead( uint32_t nAddr,  uint8_t *pData,  uint16_t usLen);

```
|**参数**|**描述**|
|:---:|:---:|
|nAddr|读取的FLASH首地址|
|pData|缓存指针|
|usLen|读取数据的长度|
|**返回值**|描述|
|无|-|

### FLASH写入数据 ###
FLASH写数据，函数原型如下所示：  
```C

void void FlashPageProgram( uint32_t nAddr,  const uint8_t *pData,  uint16_t usLen);

```
|**参数**|**描述**|
|:---:|:---:|
|nAddr|操作地址|
|pData|写入数据的指针|
|usLen|数据长度|
|**返回值**|描述|
|无|-|

### FLASH写使能 ###
FLASH写使能，函数原型如下所示：  
```C

void FlashEnableWr(void);

```

## EVTC使用示例 ##
以下提供使用UART作为事件源触发事件实例。
```C
#include <stdio.h>
#include "flash.h"

#define OPERAT_ADDR      0xA000//0x1ac00
#define OPERAT_BUF_LEN   (255)

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
	uint8_t flash_buf[OPERAT_BUF_LEN] = {0x01};
	uint8_t index = 0;
	uint32_t flash_id = 0;
	
	printf("Systerm power on.\r\n");
	delay_ms(1000);
	
	flash_id = ReadFlashID();
	printf("flash id is:0x%x\r\n", flash_id);
	
	for(index=0;index<OPERAT_BUF_LEN;index++)
	{
		flash_buf[index] = index;
	}
	
	FlashEnableWr();
	FlashEraseSector(OPERAT_ADDR);

	FlashWrite(OPERAT_ADDR, flash_buf, OPERAT_BUF_LEN);
	delay_ms(500);		/* 等待写操作完成 */
	
	FlashRead(OPERAT_ADDR, flash_buf, OPERAT_BUF_LEN);
	for(index=0; index<OPERAT_BUF_LEN; index++)
	{
		printf("%02x ", flash_buf[index]);
	}
	
	printf("\r\nread flash done.\r\n");
	delay_ms(5000);
	while(1);
	return 0;
}
```
**备注：由于core读取代码到cache中也使用SPI，用户使用SPI总线去读写FLASH时，操作应尽量简短，防止SPI冲突**
