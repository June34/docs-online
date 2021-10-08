<style>
	pre
	{
		background-color: #e8e8e8;
		white-space:pre-wrap;
	}
	td
	{
		vertical-align:middle;
	}
</style>

# GPRS指令
## AT+CGATT GPRS注册注销

<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+CGATT= &lt;state&gt;</td><td>OK<br>ERROR</td></tr><tr><td>+CGATT?</td><td>+CGATT:&lt;state&gt;</td></tr><tr><td>+CGATT=?</td><td>+CGATT:&lt;state&gt;</td></tr></table>
&lt;state&gt;: GPRS 注册状态
0:注销
1:注册
### 测试命令
<pre>发送命令：AT+CGATT=?
响    应：
+CGATT: (0,1)
OK

说明：
+CGATT: (0,1)
(0,1): 0注册 1注销</pre>
### 写命令
<pre>发送命令：AT+CGATT=1
响    应：
OK
说明：返回OK表示注册成功</pre>
### 查询命令
<pre>发送命令：AT+CGATT?
响    应：
+CGATT: 0
OK

说明:
+CGATT: 0
0: 未注册</pre>
## AT+CGACT PDP激活去激活
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+CGACT=[&lt;state&gt; [,&lt;cid&gt;[,&lt;cid&gt;[,…]]]]</td><td>OK<br>ERROR</td></tr><tr><td>+CGACT?</td><td>+CGACT: &lt;cid&gt;, &lt;state&gt;[&lt;CR&gt;&lt;LF&gt;+CGACT: &lt;cid&gt;, &lt;state&gt;[...]]</td></tr><tr><td>+CGACT=?</td><td>+CGACT:(0,1)</td></tr></table>
&lt;state&gt;指示PDP上行文激活还是去激活<br>
0:去激活<br>
1:激活<br>
<br>
&lt;cid&gt;PDP上下文标识ID，可通过+CGDCONT设置ID的PDP激活参数，未设置时使用默认PDP激活参数
###<span id=2.10.1><font color=#2f5496>测试命令<font color=bla>
<pre>发送命令：AT+CGACT=?
响    应：
+CGACT:(0,1)
OK

说明：
+CGACT:(0,1)
(0,1): 0去激活PDP, 1激活PDP</pre>
###<span id=2.10.2><font color=#2f5496>设置命令<font color=bla>
<pre>命令:+CGACT=[state][,&lt;cid&gt;[,cid..]]  
返回: OK 或者 ERROR
参数：
state必须赋值，cid可选参数
state:激活 cid:无，则激活所有已定义的PDP;
state:去激活 cid:无，则去激活所有已激活的PDP
激活时，如果MT未注册，则先注册在激活
注意：目前我们系统最大只支持2个cid编号为1,2；其它值为异常值

发送命令：AT+CGACT=1,1
响    应：
OK

说明：
AT+CGACT=1,1
1,1: 激活cid为1的PDP，返回OK表示激活成功</pre>
###<span id=2.10.3><font color=#2f5496>查询命令<font color=bla>
<pre>发送命令：AT+CGACT?
响    应：
+CGACT:1,1
OK

说明：
AT+CGACT=1,1
1,1: cid为1的PDP处于激活状态</pre>
##<span id=2.11><font color=#2f5496>AT+CGDCONT定义PDP上下文<font color=bla>
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+CGDCONT=[&lt;cid&gt; [,&lt;PDP_type&gt; [,&lt;APN&gt; [,&lt;PDP_addr&gt; [,&lt;d_comp&gt; [,&lt;h_comp&gt;]]]]]]</td><td>OK<br>ERROR</td></tr><tr><td  style="vertical-align: middle;">+CGDCONT</td><td>+CGDCONT: &lt;cid&gt;, &lt;PDP_type&gt;, &lt;APN&gt;,&lt;PDP_addr&gt;, &lt;d_comp&gt;, &lt;h_comp&gt;[,&lt;pd1&gt;[,…[,pdN]]]
<br>[&lt;CR&gt;&lt;LF&gt;+CGDCONT: &lt;cid&gt;, &lt;PDP_type&gt;, &lt;APN&gt;,&lt;PDP_addr&gt;, &lt;d_comp&gt;, &lt;h_comp&gt;[...]]</td></tr><tr><td>+CGDCONT=?</td><td>+CGDCONT: (1-2),"IP",,,(0,1),(0,1)</td></tr></table>
&lt;cid&gt;PDP上下文标识ID<br>
&lt;PDP_type&gt;PDP类型，X.25 IP IPV6 OSPIH PPP<br>
&lt;APN&gt;业务接入点，常见如cmnet cmwap ims<br>
&lt;PDP_addr&gt;指定PDP IP地址<br>
&lt;d_comp&gt;PDP数据压缩指示<br>

+ 0:off
+ 1:on
+ 2:V.42bit

&lt;h_comp&gt;PDP数据头压缩<br>

+ 0:off
+ 1:on
+ 2:RFC1144
+ 3:RFC2507

### 测试命令
<pre>发送命令：AT+CGDCONT=?
响    应：
+CGDCONT: (1-2),"IP",,,(0,1),(0,1) 
OK

说明：
+CGDCONT: cid范围,pdp_type,,,pdp数据压缩枚举值,pdp头压缩枚举值
+CGDCONT: (1-2),"IP",,,(0,1),(0,1)
(1-2): 支持两个CID 1和2
“IP”: 支持PDP类型为IP
(0,1): 支持PDP数据压缩0关闭 1打开
(0,1): 支持PDP头压缩 0关闭 1打开</pre>
### 设置命令
定义PDP context, 如PDP类型/CID等，并不发起PDP激活，如需激活需配合AT+CGACT

<pre>发送命令：AT+CGDCONT=1,"IP"
响    应：OK

说明：
AT+CGDCONT=1,"IP"
1: 设置CID为1的PDP上下文
“IP”: PDP类型为IP</pre>
### 查询命令
<pre>发送命令：AT+CGDCONT?
响    应：
+CGDCONT: 1,"IP","","",0,0   
OK

说明：
+CGDCONT: 1,"IP","","",0,0
1: 已设置CID值为1的PDP
“IP”: PDP类型为IP
0: 关闭PDP数据压缩
0: 关闭PDP头压缩</pre>
## AT+CGQREQ设置QOS
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+CGQREQ=[&lt;cid&gt; [,&lt;precedence &gt; [,&lt;delay&gt; [,&lt;reliability.&gt; [,&lt;peak&gt; [,&lt;mean&gt;]]]]]]</td><td>OK<br>ERROR</td></tr><tr><td  style="vertical-align: middle;">+CGQREQ?</td><td>+CGQREQ: &lt;cid&gt;, &lt;precedence &gt;, &lt;delay&gt;, &lt;reliability&gt;, &lt;peak&gt;, &lt;mean&gt;<br>
[&lt;CR&gt;&lt;LF&gt;+CGQREQ: &lt;cid&gt;, &lt;precedence &gt;, &lt;delay&gt;, &lt;reliability.&gt;, &lt;peak&gt;, &lt;mean&gt;
[…]]</td></tr><tr><td>+CGQREQ=?</td><td>+CGQREQ: "IP",(1-3),(1-4),(1-5),(1-9),(1-18,31)</td></tr></table>
在PDP激活前，设置PDP激活请求QOS参数值，是对AT+CGDCONT命令(PDP上下文定义)补充
&lt;cid&gt;PDP上下文标识ID<br>
&lt;PDP_type&gt;PDP类型，X.25 IP IPV6 OSPIH PPP<br>
&lt;precedence&gt;优先级<br>

+ 0:预定的
+ 1:高优先级
+ 2:正常优先级
+ 3:低优先级

&lt;delay&gt;时延

+ 0:默认值
+ 1~3:QOS延时级别

&lt;reliability&gt;可靠性

+ 0:默认值
+ 1:LLC ack, LLC data prot, RLC ack, GTP ack
+ 2:LLC ack, LLC data prot, RLC ack, GTP unack
+ 3:LLC unack, LLC data prot, RLC ack GTP unack
+ 4:LLC unack, LLC data prot, RLC unack GTPunack
+ 5:LLC unack, no LLC data prot, RLC unack

&lt;peak&gt;峰值吞吐量

+ 0:默认值
+ 1~9:峰值吞吐量级别

&lt;mean&gt;平均吞吐量

+ 0:默认值
+ 1~18:平均吞吐量级别
+ 31:尽力而为的平均吞吐量级别

### 测试命令
<pre>发送命令：AT+CGQREQ=?
响    应：
+CGQREQ: "IP",(1-3),(1-4),(1-5),(1-9),(1-18,31)  
OK   

说明：
+CGQREQ: "IP",(1-3),(1-4),(1-5),(1-9),(1-18,31)
“IP”: 只支持IP的PDP类型
(1-3): QOS优先级，详见GSM03.60
(1-4): QOS时延，详见GSM03.60
(1-5): QOS可靠性，详见GSM03.60
(1-9): QOS峰值吞吐量，详见GSM03.60
(1-18,31): QOS平均吞吐量, 详见GSM03.60

GSM 03.60
precedence: 优先级
delay: 时延
reliability: 可靠性
peak: 峰值吞吐量
mean: 平均吞吐量</pre>
### 设置命令
只设置cid时，则对于cid PDP上行文用默认QOS
<pre>发送命令：AT+CGQREQ=1,1,4,5,2,14
响    应：
OK</pre>
### 查询命令
查询当前PDP QOS设置值；注意只能查询到未激活的PDP QOS值
<pre>发送命令：AT+CGQREQ?
响    应：
+CGQREQ: 1,0,0,0,0,0
OK</pre>
## AT+CGQMIN设置PDP最小QOS
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+CGQMIN=[&lt;cid&gt; [,&lt;precedence &gt; [,&lt;delay&gt; [,&lt;reliability.&gt; [,&lt;peak&gt; [,&lt;mean&gt;]]]]]]</td><td>OK<br>ERROR</td></tr><tr><td  style="vertical-align: middle;">+CGQMIN?</td><td>+CGQMIN: &lt;cid&gt;, &lt;precedence &gt;, &lt;delay&gt;, &lt;reliability&gt;, &lt;peak&gt;, &lt;mean&gt;<br>
[&lt;CR&gt;&lt;LF&gt;+CGQMIN: &lt;cid&gt;, &lt;precedence &gt;, &lt;delay&gt;, &lt;reliability.&gt;, &lt;peak&gt;, &lt;mean&gt;
[…]]</td></tr><tr><td>+CGQMIN=?</td><td>+CGQMIN: "IP",(1-3),(1,4),(1-5),(1-9),(1-18,31)</td></tr></table>
### 测试命令
<pre>发送命令：AT+CGQMIN=?
响    应：
+CGQMIN: "IP",(1-3),(1-4),(1-5),(1-9),(1-18,31)  
OK   

说明：
+CGQMIN: "IP",(1-3),(1-4),(1-5),(1-9),(1-18,31)
“IP”: 只支持IP的PDP类型
(1-3): QOS优先级，详见GSM03.60
(1-4): QOS时延，详见GSM03.60
(1-5): QOS可靠性，详见GSM03.60
(1-9): QOS峰值吞吐量，详见GSM03.60
(1-18,31): QOS平均吞吐量, 详见GSM03.60

GSM 03.60
precedence: 优先级
delay: 时延
reliability: 可靠性
peak: 峰值吞吐量
mean: 平均吞吐量</pre>
### 设置命令
<pre>发送命令：AT+CGQMIN=1,1,4,5,2,14
响    应：
OK</pre>
### 查询命令
<pre>发送命令：AT+CGQMIN?
响    应：
+CGQMIN: 1,0,0,0,0,0
OK</pre>
## AT+CGPADDR显示PDP地址
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+CGPADDR=[&lt;cid&gt; [,&lt;cid&gt; [,…]]]</td><td>+CGPADDR: &lt;cid&gt;,&lt;PDP_addr&gt;<br>[&lt;CR&gt;&lt;LF&gt;+CGPADDR: &lt;cid&gt;,&lt;PDP_addr&gt;[...]]</td></tr><tr><td>+CGPADDR=?</td><td>+CGPADDR:(1,2)</td></tr></table>
### 测试命令
<pre>发送命令：AT+CGPADDR=?
响    应：
+CGPADDR: (1,2)  
OK

说明：
+CGPADDR: (1,2)  
(1,2): 当前已定义cid 1和2的PDP</pre>
### 设置命令
<pre>发送命令：AT+CGPADDR=1,2
响    应：
+CGPADDR: 1,"192.168.40.13"
+CGPADDR: 2,"192.168.40.14"
OK

说明：
+CGPADDR: 1,"192.168.40.13"
1: cid为1的PDP
"192.168.40.13": PDP IP地址</pre>
## AT+CGREG网络注册状态
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+CGREG=[&lt;n&gt;]</td><td>OK<br>ERROR</td></tr><tr><td>+CGREG?</td><td>+CGREG: &lt;n&gt;,&lt;stat&gt;[,&lt;lac&gt;,&lt;ci&gt;]
+ERROR: &lt;err&gt;</td></tr><tr><td>+CGREG=?</td><td>+CGREG:&lt;n&gt;</td></tr></table>
当MT的GPRS网络注册状态变化时，是否主动上报+CGREG=&lt;stat&gt;注册状态值。

参数说明<br>
&lt;n&gt;

+ 0: 禁止主动上报
+ 1：使能主动上报

&lt;stat&gt;

+ 0: 未注册，MT当前不在搜索注册一个新的运营商
+ 1：已注册，在本地网络
+ 2：未注册，MT当前正在搜索注册一个新的运营商
+ 3：拒绝注册
+ 4：位置
+ 5：已注册，漫游中

### 测试命令
<pre>发送命令：AT+CGREG=?
响    应：
+CGREG: (0-1)
OK

说明：
+CGREG: (0-1)
(0-1): 0禁止主动上报 1:使能主动上报</pre>
### 设置命令
<pre>发送命令：AT+CGREG=1
响    应：
OK

说明：
AT+CGREG=1
1: 当MT的GPRS网络注册状态变化时，主动上报+CGREG=&lt;stat&gt;注册状态值</pre>
### 查询命令
<pre>发送命令：AT+CGREG?
响    应：
+CGREG: 0,0
OK

说明：
+CGREG: 0,0
0,0: 0不主动上报，0未注册</pre>
## AT+CGDATA进入数据态
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+CGDATA=[&lt;L2P&gt; ,[&lt;cid&gt; [,&lt;cid&gt; [,…]]]]</td><td>CONNECT<br>ERROR</td></tr><tr><td>+CGDATA=?</td><td>+CGDATA:&lt;L2P&gt;</td></tr></table>
AT命令态切换到数据态，开始传输数据，在此期间其他AT指令无效，要等数据传送完且由数据态切换到命令态成后才能继续AT指令。执行该命令相当于完成attach和activePDP。
&lt;L2P&gt;TE和MT间层2协议，常见如PPP PAD X25
### 测试命令
<pre>发送命令：AT+CGDATA=?
响    应：
+CGDATA: "PPP"
OK

说明：
+CGDATA: "PPP"
“PPP”: 在TE和MT之间使用PPP协议</pre>
### 设置命令
所有参数都是可选参数，不配置时选择默认PDP
L2P配置时，只能配置PPP
cid配置时，指定对应的PDP
<pre>发送命令：AT+CGDATA=”PPP”,1
响    应：
CONNECT

说明：
AT+CGDATA=”PPP”,1
“PPP”: L2P协议PPP
1: cid为1的PDP</pre>
## AT+CSQ检测信号强度
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+CSQ</td><td>+CSQ: &lt;rssi&gt;,&lt;ber&gt;<br>
+ERROR: &lt;err&gt;</td></tr><tr><td>+CSQ=?</td><td>+CSQ: &lt;rssi&gt;,&lt;ber&gt;</td></tr></table>
&lt;rssi&gt;接收信号强度

+ 0 &emsp;&emsp;&emsp;&emsp;        <=-113dBm
+ 1     &emsp;&emsp;&emsp;      &emsp;-111dBm
+ 2-30  &emsp;&emsp;&ensp; -109到-53dBm
+ 31   &emsp;&emsp;&emsp;&ensp;   -51dBm or greater
+ 99   &emsp;&emsp;&emsp;&ensp;   未知或不可检测

&lt;ber&gt;信道误码率, 0~7 表示误码率由低到高,99 表示未知.

+ 0 BER < 0.2%
+ 1 0.2% < BER < 0.4%
+ 2 0.4% < BER < 0.8%
+ 3 0.8% < BER < 1.6%
+ 4 1.6% < BER < 3.2%
+ 5 3.2% < BER < 6.4%
+ 6 6.4% < BER < 12.8%
+ 7 12.8% < BER
+ 99 未知或不可测

### 测试命令
<pre>发送命令：AT+CSQ=?
响    应：
+CSQ: (2-31,99),(99)
OK

说明：
+CSQ: (2-31,99),(99)
(2-31,99): 2-30 rssi-109到-53dBm, 99 rssi未知或不可检测, 99 ber未知</pre>
### 执行命令
发送命令：AT+CSQ
响    应：
+CSQ: 21,99
OK

说明：21 rssi为-71dBm, 99 ber未知
## AT+COPS运营商选择
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+COPS=[&lt;mode&gt;[,&lt;format&gt;[,&lt;oper&gt;]]]</td><td>+ERROR: &lt;err&gt; 或者 正确可用的网络</td></tr><tr><td style="vertical-align: middle;">+COPS?</td><td>+COPS: &lt;mode&gt;[,&lt;format&gt;,&lt;oper&gt;]<br>+ERROR: &lt;err&gt;</td></tr><tr><td style="vertical-align: middle;">+COPS=?</td><td>+COPS: [list of supported (&lt;stat&gt;,long alphanumeric &lt;oper&gt;,short alphanumeric &lt;oper&gt;,numeric &lt;oper&gt;)s]
[,,(list of supported &lt;mode&gt;s),(list of supported &lt;format&gt;s)]
+ERROR: &lt;err&gt;;</td></tr></table>
搜索网络，设置注册网络，

&lt;mode&gt;运营商选择模式

+ 0-自动  忽略format,oper
+ 1-手动  format,oper必须有
+ 2-强制注销网络 
+ 3-设置查询格式(用于+cops?返回格式) 
+ 4-手动/自动  因为有手动 oper自动必须有，如果手动强制失败，则进入自动模式

&lt;format&gt;运营商名字表示方式

+ 0:长字符串，如chinaMobile
+ 1:短字符串，如cmee
+ 2:数组，如46000

&lt;stat&gt;运营商可用状态

+ 0 unknown 
+ 1 available 可用的运营的
+ 2 current 当前SIM卡正在使用的PLMN
+ 3 forbidden 禁止PLMN

### 测试命令
搜索PLMN，列出当前搜索到的PLMN，并告诉哪些运营商可用;只搜网不注册。
返回: [stat,"长串","短串",Num编号],,支持的mode,支持的format
<pre>发送命令：AT+COPS=?
响    应：
+COPS: (1,"","","26242"),(0,"","","46242")   
OK

说明:
(1,"","","26242")
1: 搜索到的运营商可用
“”: 运营商长字符串为空
“”: 运营商短字符串为空
“26242”: 运营商数字编号

(0,"","","46242")
0: 搜索到的运营商未知
“”: 运营商长字符串为空
“”: 运营商短字符串为空
“46242”: 运营商数字编号</pre>
### 设置命令
空闲态下强制设置运营商并注册到运营商网络。
AT+COPS=mode[,format[,oper]] 设置配置命令,
<pre>发送命令：AT+COPS=1,2,"46000"
响    应：
OK

说明：
AT+COPS=1,2,"46000"
1: 手动选择
2: 运营商使用数字编号
“46000”: 运营商数字编号

发送命令：AT+COPS=1,2,"26242"
响    应：
+COPS: (1,"","","46000")
OK</pre>
说明：强制注册到26242运营商网络，但注册失败返回可用的PLMN列表，ms处于未注册态。
### 查询命令
<pre>发送命令：AT+COPS?
响    应：
+COPS: 0,2,"26242"
OK

说明：
+COPS: 0,2,"26242"  
0:自动选择运营商模式  
2:选择的运营商用数字表示  
“26242”: 选择的运营商</pre>
## AT+LPM 低功耗控制
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+LPM= [&lt;mode&gt;],[&lt;control&gt;]</td><td>OK<br>ERROR</td></tr><tr><td>+LPM?</td><td>+LPM: &lt;mode&gt;,&lt;control&gt;</td></tr><tr><td>+LPM=?</td><td>+LPM: &lt;mode&gt;,&lt;control state&gt;</td></tr></table>
用于低功耗控制，主要有normal 、sleep、retention 3种模式，默认状态为normal，不进入低功耗：<br>
&lt;mode&gt;: GPRS 工作模式，主要用于功耗控制

+ 0:sleep模式，芯片进入睡眠，外部中断唤醒后重新启动
+ 1:retention模式，GPRS根据基站的通信选择空闲状态进入睡眠，有任务后继续工作，外部中断可以正常唤醒。串口中断唤醒后在1S内不会进入retention模式（串口数据触发中断唤醒会造成数据丢失，如果是通过AT唤醒，则需要连续发送2个AT命令，第一个AT由于部分数据丢失会返回ERROR，在1S内第二个AT能正常执行）。

&lt;control&gt;: 控制功耗开关

+ 0：打开。允许进入低功耗。
+ 1：关闭。不允许进入低功耗。