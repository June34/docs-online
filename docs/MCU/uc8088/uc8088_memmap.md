## 存储器映像

所有UC8088xxx中存储器和外设的地址映像如下表所示：



<table>
   <tr>
       <td>预定义</td>
       <td>起始地址</td>
       <td>外设</td>
       <td>总线</td>
	</tr>
	<tr>
       <td>启动代码</td>
       <td>0x0000 0000-0x0000 2FFF</td>
       <td>Boot loader </td>
       <td> </td>
	</tr>
	<tr>
       <td>主要程序</td>
       <td>0x0000 3000-0x0020 0000</td>
       <td>Main Flash </td>
       <td> </td>
	</tr>
	<tr>
       <td>数据Rom</td>
       <td>0x0020 0000-0x0010 0000</td>
       <td>Data Rom</td>
       <td> </td>
	</tr>
	<tr>
       <td rowspan="2">数据内存</td>
       <td>0x0030 0000-0x0033 FFFF</td>
       <td>Data Ram</td>
       <td> </td>
	</tr>
	<tr>
       <td>0x0034 0000-0x1A0F FFFF</td>
       <td>保留</td>
       <td> </td>
	</tr>
	<tr>
       <td rowspan="24">外设</td>
       <td>0x1A10 0000-0x1A10 001F</td>
       <td>UART1</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 0020-0x1A10 0FFF</td>
       <td>UART2</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 1000-0x1A10 1FFF</td>
       <td>GPIO</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 2000-0x1A10 2FFF</td>
       <td>SPIM</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 3000-0x1A10 300F</td>
       <td>TIMER0</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 3010-0x1A10 3FFF</td>
       <td>TIMER1</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 4000-0x1A10 41FF</td>
       <td>EVENT</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 4200-0x1A10 42FF</td>
       <td>WATCHDOG</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 4300-0x1A10 4FFF</td>
       <td>RTC</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 5000-0x1A10 5FFF</td>
       <td>I2C</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 6000-0x1A10 6FFF</td>
       <td>FLL</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 7000-0x1A10 7FFF</td>
       <td>SOC_CTRL</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 8000-0x1A10 8FFF</td>
       <td>SIM</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 9000-0x1A10 9FFF</td>
       <td>ADC/DAC</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 A000-0x1A10 AFFF</td>
       <td>PMU</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 B000-0x1A10 BFFF</td>
       <td>PWM</td>
       <td>APB</td>
	</tr>
	<tr>
       <td>0x1A10 C000-0x1A10 CFFF</td>
       <td>XIP</td>
       <td>APB</td>
	</tr>
</table>
