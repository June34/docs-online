<style>
	pre
	{
		background-color: #e8e8e8;
		white-space:pre-wrap;
	}
	table
	{
	vertical-align: middle;
	}
	tr
	{
	vertical-align: middle;
	}
	td
	{
	vertical-align: middle;
	}
</style>




# 基础指令
## 设定模块功能

该命令用于选择MT功能，当选择全功能时消耗最大功率，当选择最小功能时消耗最小功率。

<table ><tr><td width=99999>Command</td><td width=65%>Possible response(s)</td></tr><tr><td >+CFUN=?</td><td>+CFUN: (0,1),(0)</td></tr><tr><td >+CFUN?</td><td>+CFUN:&lt;fun&gt;,&lt;rst&gt;<br>+ERROR: &lt;err&gt;</td></tr><tr><td>+CFUN=[&lt;fun&gt;[,&lt;rst&gt;]]</td><td>+ERROR: &lt;err&gt;</td></tr></table>

&lt;fun&gt;功能选择<br>

+ 0:最小功能
+ 1:全功能

&lt;rst&gt;是否需要复位
0:不复位
1:复位

### 测试命令

<pre  style="background-color: #e8e8e8">
发送命令：AT+CFUN=?
响    应：
+CFUN: (0,1),(0)
OK
说明：
+CFUN: (0,1),(0)
(0,1): 0支持最小功能，1支持全功能
(0): 在设置设备上电前不复位MT，保持默认值即可
</pre>

### 设置命令
<pre style="background-color: #e8e8e8">
发送命令：AT+CFUN=1
响    应：
OK
说明：设置全功能，设备上电
</pre>
### 查询命令
<pre style="background-color: #e8e8e8">
发送命令：AT+CFUN?
响    应：
+CFUN: 1
OK

说明：
+CFUN: 1
1: 设备已上电，设置在全功能模式
</pre>
## AT+CGMI 读取厂商名
<font color=bla>
<table><tr><td width=99999>Command</td><td width=65%>Possible response(s)</td></tr><tr><td style=“text-align:center;vertical-align:middle;”>+CGMI</td><td>&lt;manufacturer&gt;<br>+ERROR: &lt;err&gt;</td></tr></table>

### 执行命令
<pre style="background-color: #e8e8e8">
发送命令：AT+CGMI
响    应：
ucchip
OK
</pre>

## AT+CGMM读取设备名

<table><tr><td width=99999>Command</td><td width=65%>Possible response(s)</td></tr><tr><td style=“text-align:center;vertical-align:middle;”>+CGMM</td><td>&lt;model&gt;<br>+ERROR: &lt;err&gt;</td></tr></table>

### 执行命令
<pre style="background-color: #e8e8e8">
发送命令：AT+CGMM
响    应：
8088
OK
</pre>

## AT+CGMR读取设备版本信息
<table><tr><td width=99999>Command</td><td width=65%>Possible response(s)</td></tr><tr><td style=“text-align:center;vertical-align:middle;”>+CGMR</td><td>&lt;revision&gt;<br>+ERROR: &lt;err&gt;</td></tr><tr></table>

### 执行命令
<pre style="background-color: #e8e8e8">
发送命令：AT+CGMR
响    应：
+SOFTV:012345678
+HARDV:00123456
+MANUF:ucchip
OK
</pre>

## AT+CGSN读取IMEI

<table><tr><td width=99999>Command</td><td width=65%>Possible response(s)</td></tr><tr><td style=“text-align:center;vertical-align:middle;”>+CGSN</td><td>&lt;sn&gt;<br>+ERROR: &lt;err&gt;</td></tr></table>

### 执行命令
<pre style="background-color: #e8e8e8">
发送命令：AT+CGSN
响    应：
863711020146740
OK
</pre>

## AT+CIMI读取IMSI

<table><tr><td width=99999 style=“text-align:center;vertical-align:middle;”>Command</td><td width=65%>Possible response(s)</td></tr><tr><td style=“text-align:center;vertical-align:middle;”>+CIMI</td><td>&lt;IMSI&gt;<br>+ERROR: &lt;err&gt;</td></tr></table>

### 执行命令
<pre style="background-color: #e8e8e8">发送命令：AT+CIMI
响    应：
460040812002376
OK</pre>

##<span id=2.7><font color=#2f5496>AT+CMEE设置日志输出明细<font color=bla>

<table><tr><td width=12345>Command</td><td width =65%>Possible response(s)</td></tr><tr><td>+CMEE=[&lt;n&gt;]</td><td>OK</td></tr><tr><td>+CMEE?</td><td>+CMEE:&LT;n&gt;</td></tr><tr><td style=“text-align:center;vertical-align:middle;”>+CMEE=?</td><td>+CMEE:(0-2)<br>OK</td></tr></table>

### 测试命令<font color=bla>

<pre>
发送命令：AT+CMEE=?
响    应：
+CMEE: (0-2)
OK
说明:
+CMEE: (0-2)
(0-2):
0表示关闭模块的出错报告,当模块的 AT 执行错误时,返回值仅为"ERROR"
1表示开启模块的出错报告,当模块的 AT 执行错误时,返回值为"+CME ERROR: NUM "或者"+EXT ERROR: NUM ", NUM 代表错误代码,依照此代码可查询错误类型，可附表3.1CMEE错误码和3.2EXT错误码
2表示开启模块的出错报告,当模块的 AT 执行错误时,返回值为"+CME ERROR: verbose "或者"+EXT ERROR: NUM " ,verbose 代表错误细节
注意：本文档+ERROR指+CME ERROR或者+EXT ERROR
</pre>

###<span id=2.7.2><font color=#2f5496>设置命令<font color=bla>
<pre>设置日志输出类型
发送命令：AT+CMEE=2
响    应：
OK</pre>

###<span id=2.7.3><font color=#2f5496> 查询命令<font color=bla>
<pre>查询当前日志输出类型配置
发送命令：AT+CMEE?
响    应：
+CMEE: 2
OK</pre>

##<span id=2.8><font color=#2f5496>AT+CELLLOCK 锁频<font color=bla>
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+CELLLOCK= [&lt;state&gt;],[&lt;freq&gt;]</td><td>OK<br>ERROR</td></tr><tr><td>+CELLLOCK?</td><td>+CELLLOCK: &lt;state&gt;,&lt;freq&gt;</td></tr><tr><td>+CELLLOCK=?</td><td>+CELLLOCK: (list of supported &lt;state&gt;s),freq</td></tr></table>
锁定MS到固定频率，注意只能在未注册状态才能锁频成功，支持的锁频频率范围：
E-GSM900 DCS1800 PCS1900

&lt;state&gt;: 锁屏操作类型
0:取消锁屏
1:锁屏
&lt;freq&gt;: 频率值HZ
###<span id=2.8.1><font color=#2f5496>测试命令<font color=bla>
<pre>发送命令：AT+CELLLOCK=?
响    应：
+CELLLOCK: (0,1),freq
OK

说明：
+CELLLOCK: (0,1)
(0,1): 0取消锁频 1锁频
freq: 频率值</pre>

###<span id=2.8.2><font color=#2f5496> 设置命令<font color=bla>
<pre>发送命令：AT+CELLLOCK=1,935400000
响    应：
OK
说明：返回OK表示锁屏设置成功</pre>

###<span id=2.8.3><font color=#2f5496> 查询命令<font color=bla>
<pre>发送命令：AT+ CELLLOCK?
响    应：
+CELLLOCK: 1,935400000
OK

说明:
+CELLLOCK: 1, 935400000
1: 锁频
935400000: 锁定频率935400000HZ</pre>
