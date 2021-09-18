# 外设控制器

## 外设控制器简介

外设控制单元实现对外设的基本控制。通过配置相应的寄存器，可以定义GPIO复用功能、外设时钟Gating状态、使能代码解密模块、使能CCE取指、GPIO上下拉配置、以及EFUSE烧写控制等。

外设控制器主要特征：

- 配置GPIO的复用功能、上下拉器
- 控制所有APB外设的时钟Gating
- 使能代码解密模块
- 使能CCE取指令
- EFUSE烧写控制

## GPIO复用功能配置

GPIO功能复用，配置PAD_MUX寄存器对应的位，可将IO复用为其它特定的功能。复用功能对照表如下：

| GPIO   | 复用功能         | GPIO   | 复用功能           |
| ------ | ---------------- | ------ | ------------------ |
| GPIO3  | I2C, SCL         | GPIO17 | UART2, UART_RTS    |
| GPIO4  | I2C, SDA         | GPIO18 | UART2, UART_DTR    |
| GPIO5  | SMARTCARD, TX    | GPIO19 | UART2, UART_CTS    |
| GPIO6  | SMARTCARD, CLK   | GPIO20 | UART2, UART_DSR    |
| GPIO7  | SMARTCARD, RST   | GPIO21 | UART2, UART_AUX_TX |
| GPIO8  | SPI MASTER, CLK  | GPIO22 | UART2, UART_AUX_RX |
| GPIO9  | SPI MASTER, SDO0 | GPIO26 | PWM, PWM_OUT0      |
| GPIO10 | SPI MASTER, SDO1 | GPIO27 | PWM, PWM_OUT1      |
| GPIO11 | SPI MASTER, SDO2 | GPIO28 | PWM, PWM_OUT2      |
| GPIO12 | SPI MASTER, SDO3 | GPIO29 | PWM, PWM_OUT3      |
| GPIO13 | SPI MASTER, CSN0 |        |                    |
| GPIO14 | SPI MASTER, CSN1 |        |                    |
| GPIO15 | SPI MASTER, CSN2 |        |                    |
| GPIO16 | SPI MASTER, CSN3 |        |                    |

## 代码解密和CCE使能

若软件代码为加密烧录，则必须使能代码解密模块，此时需要配置MTR_CFG0寄存器对应位为使能状态；反之，若软件代码为非加密烧录，则必须除能解密模块，此时需要配置MTR_CFG0寄存器对应位为除能状态。类似，若要使能/除能CCE取指功能，此时需要配置CCE_FETCH寄存器对应位。