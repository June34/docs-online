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

| GPIO   | 复用功能         | GPIO   | 复用功能        |
| ------ | ---------------- | ------ | --------------- |
| GPIO0  | SPI SLAVE, SDI0  | GPIO17 | UART2, UART_RTS |
| GPIO1  | SPI SLAVE, SDI1  | GPIO18 | UART2, UART_DTR |
| GPIO2  | SPI SLAVE, SDI2  | GPIO19 |                 |
| GPIO3  | SPI SLAVE, SDI3  | GPIO20 |                 |
| GPIO4  | SPI SLAVE, CLK   | GPIO21 |                 |
| GPIO5  | I2C,SCL          | GPIO22 |                 |
| GPIO6  | I2C,SDA          | GPIO23 |                 |
| GPIO7  | AP CLK SYNC      | GPIO24 |                 |
| GPIO8  | SPI MASTER, CLK  | GPIO25 |                 |
| GPIO9  | SPI MASTER, SDO0 | GPIO26 |                 |
| GPIO10 | SPI MASTER, SDO1 | GPIO27 |                 |
| GPIO11 | SPI MASTER, SDO2 | GPIO28 |                 |
| GPIO12 | SPI MASTER, SDO3 | GPIO29 |                 |
| GPIO13 | SPI MASTER, CSN0 | GPIO30 |                 |
| GPIO14 | UART1, TX        | GPIO31 |                 |
| GPIO15 | SPI MASTER, CSN1 |        |                 |
| GPIO16 | SPI MASTER, CSN2 |        |                 |

配置PAD_MUX1寄存器对应的位，可将IO复用为其它特定的功能。复用功能对照表如下：

| GPIO   | 复用功能              | GPIO   | 复用功能     |
| ------ | --------------------- | ------ | ------------ |
| GPIO0  | SPI MASTER, CLK       | GPIO17 | PWM,PWM_OUT2 |
| GPIO1  | SPI MASTER, CSN       | GPIO18 | PWM,PWM_OUT3 |
| GPIO2  | SPI MASTER, SDI       | GPIO19 |              |
| GPIO3  | SPI MASTER, SDO       | GPIO20 |              |
| GPIO4  | I2C, SCL              | GPIO21 |              |
| GPIO5  | RX_OUT_I_IO(for test) | GPIO22 |              |
| GPIO6  | RX_OUT_Q_IO(for test) | GPIO23 |              |
| GPIO7  | RX CLK(for test)      | GPIO24 |              |
| GPIO8  | CLK32(for test)       | GPIO25 | CANBUS,TX    |
| GPIO9  | DCXO CLK(for test)    | GPIO26 | CANBUS,TX    |
| GPIO10 | CAN BUS,TX            | GPIO27 |              |
| GPIO11 | CAN BUS,RX            | GPIO28 |              |
| GPIO12 | UART1,UART_TX         | GPIO29 |              |
| GPIO13 | UART1,UART_RX         | GPIO30 |              |
| GPIO14 | I2C,SDA               | GPIO31 |              |
| GPIO15 | PWM,PWM_OUT0          |        |              |
| GPIO16 | PWM,PWM_OUT1          |        |              |

## 代码解密和CCE使能

若软件代码为加密烧录，则必须使能代码解密模块，此时需要配置MTR_CFG0寄存器对应位为使能状态；反之，若软件代码为非加密烧录，则必须除能解密模块，此时需要配置MTR_CFG0寄存器对应位为除能状态。类似，若要使能/除能CCE取指功能，此时需要配置CCE_FETCH寄存器对应位。