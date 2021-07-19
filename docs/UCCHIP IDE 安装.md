# UCCHIP IDE 安装

UCCHIP IDE当前提供Windows 10 64位操作系统和Ubuntu 64位操作系统的安装版本，如果对其他系统有诉求，请联系我们。

## 获取安装文件

###### Windows 

获取UCCHIP IDE安装包：[UCCHIP IDE Windows](https://uc8088.com/t/topic/49)

###### Ubuntu 

获取UCCHIP IDE 安装包 : [UCCHIP IDE Ubuntu](https://uc8088.com/t/topic/49)

## 安装指导

###### Windows 

Windows环境包含了RISCV GCC编译器 双击UCCHIP-IDE-Setup-WIN-x64-X.XX.X.exe ,依照屏幕提示，安装集成开发环境。

<img src="C:\Users\jqiu\Desktop\rfid\E7F95C75-5E91-470a-9632-3359AB023398.png" alt="E7F95C75-5E91-470a-9632-3359AB023398" style="zoom: 67%;" />



使用默认，或者设置程序的安装位置，点击下一步。

<img src="C:\Users\jqiu\Desktop\rfid\AA055358-1E62-44a3-BF02-E5367C314560.png" style="zoom: 67%;" />

安装完成后，界面如下：

<img src="C:\Users\jqiu\Desktop\rfid\71F6CF41-906A-4198-BFB7-9BF766F821D6.png" style="zoom: 67%;" />

###### Ubuntu

安装UCCHIP IDE依赖环境：

> sudo apt-get install libftdi-dev

安装UCCHIP-IDE-Setup-Ubuntu-x64-X.XX.X.deb

> sudo dpkg - i UCCHIP-IDE-Setup-Ubuntu-x64-X.XX.X.deb

正确安装完成后，界面如下：

<img src="C:\Users\jqiu\Desktop\rfid\273E268A-C290-4489-B361-ADFE3FEE7546.png" style="zoom:67%;" />



## 仿真器设备驱动

UCCHIP系列芯片采用UC-DAP5仿真器，需要安装串口驱动和调试口驱动。

###### Windows

双击CDM21228_Setup.exe,依照屏幕提示，安装串口驱动。

<img src="C:\Users\jqiu\Desktop\rfid\9ECCB481-87AF-4a21-A281-194273DD59B7.png" style="zoom:67%;" />

获取ftdi驱动，一直点击下一步。

<img src="C:\Users\jqiu\Desktop\rfid\E70EBA13-C09A-4626-A1BD-FF21BF2AD15D.png" style="zoom:67%;" />

安装完成后，界面如下：

<img src="C:\Users\jqiu\Desktop\rfid\708598D1-5E8E-4d62-B347-1D0BAD0F30CB.png" style="zoom:67%;" />

双击UC_DAP_Setup.exe,依照屏幕提示，安装串口驱动。

<img src="C:\Users\jqiu\Desktop\rfid\9D73EB60-32A4-45cd-B573-2C1CC563B17E.png" style="zoom:67%;" />



点击下一步，安装完成后，界面如下：

<img src="C:\Users\jqiu\Desktop\rfid\DE3C2396-5622-4f76-834B-B74278F71851.png" style="zoom:67%;" />

###### Ubuntu

Ubuntu安装依赖时，已经完成了调试口和串口的安装，所以不需要额外安装驱动。



## 安装riscv-unknown-elf软件

UCCHIP IDE使用 riscv-gcc 编译器进行编译，需要安装编译器软件。Windows安装包已经集成了编译器软件，Ubuntu需要下载和添加到IDE中。

###### Ubuntu

获取riscv-gcc编译器：[riscv-gcc](https://uc8088.com/t/topic/49)

添加riscv-gcc编译器到UCCHIP IDE，依照屏幕提示：

从菜单栏中，依次打开 **Setting** —> **Build Settings**，点击 “**+**”按钮

<img src="C:\Users\jqiu\Desktop\rfid\1D50DD96-7525-44ef-B4B6-059A214B5D08.png" style="zoom:67%;" />

选中riscv-gcc软件中**bin**目录

<img src="C:\Users\jqiu\Desktop\rfid\98018768-8CD6-4e98-BF0A-C06E3ED59476.png" style="zoom:67%;" />

点击Open，添加完成后，修改编译器名称，如图

<img src="C:\Users\jqiu\Desktop\rfid\96C8E855-589F-464e-B974-F5688C586BED.png" style="zoom:67%;" />

编译器添加完成后，界面如下：

<img src="C:\Users\jqiu\Desktop\rfid\450C3DCC-FADE-4c0f-9824-F7C293D6C37D.png" style="zoom:67%;" />







