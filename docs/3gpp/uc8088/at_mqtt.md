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
# MQTT相关命令
## AT+MQTTTLS设置MQTT的TLS参数
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+MQTTTLS=&lt;tls&gt;,&lt;type&gt;,&lt;cert file content&gt;,&lt;cert file size&gt;</td><td>OK<br>ERROR</td></tr><tr><td>+HTTPREAD?</td><td>+MQTTTLS:(0-1),&lt;type&gt;,&lt;cert file content&gt;,&lt;cert file size&gt;</td></tr><tr><td>+MQTTTLS=?</td><td>+MQTTTLS:&lt;tls&gt;,&lt;type&gt;,&lt;cert file content&gt;,&lt;cert file size&gt;</td></tr></table>
&lt;tls&gt;:tls加密使能标志

+ 0:tcp
+ 1:tls

&lt;type&gt;:证书类型

+ 0:根证书名
+ 1:客户端证书名
+ 2:客户端私钥名

&lt;cert file content&gt;:(预存的)证书内容，最多1536字节<br>
&lt;cert file size &gt;:(预存的)指示证书数据长度，不超过1536

###测试命令
<pre>
发送:AT+MQTTTLS=?
响    应:
+MQTTTLS:(0-1),&lt;type&gt;,&lt;cert file content&gt;,&lt;cert file size&gt;
</pre>
###设置命令
<pre>
发送:AT+MQTTTLS=1,0,"-----BEGIN CERTIFICATE-----......",1181
响    应:
OK
</pre>
###查询命令
<pre>
发送:AT+MQTTTLS?
响    应:
+MQTTTLS:1
OK
</pre>

## AT+MQTTCONFIG设置MQTT相关参数

<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+MQTTCONFIG=&lt;url&gt;,&lt;port&gt;,&lt;client id&gt;,&lt;cleansession&gt;,&lt;keepalive&gt;,&lt;username&gt;,&lt;password&gt;</td><td  style="vertical-align: middle;">OK<br>ERROR</td></tr><tr><td  style="vertical-align: middle;">+HTTPCONFIG?</td><td>+MQTTTLS:&lt;url&gt;,&lt;port&gt;,&lt;client id&gt;,(0-1),&lt;keepalive&gt;,&lt;username&gt;,&lt;passworkd&gt;</td></tr><tr><td  style="vertical-align: middle;">+MQTTCONFIG=?</td><td>+MQTTCONFIG:&lt;url&gt;,&lt;port&gt;,&lt;client id&gt;,(0-1),&lt;keepalive&gt;,&lt;username&gt;,&lt;password&gt;</td></tr></table>
&lt;url&gt;:服务器IP地址或者域名<br>
&lt;port&gt;:服务器端口号<br>
&lt;client id&gt;:客户端标识<br>
&lt;clean session&gt;:新建会话选项

+ 0:保持原会话
+ 1:新建会话

&lt;keepalive&gt;:发送数据最大间隔时间
&lt;username&gt;:用户名
&lt;password&gt;:密码
### 测试命令
<pre>
发送:AT+MQTTCONFIG=?
响    应:
+MQTTCONFIG:&lt;url&gt;,&lt;port&gt;.&lt;clientid&gt;,(0-1),&lt;keepalive&gt;,&lt;username&gt;,&lt;password&gt;
OK
</pre>
### 设置命令
<pre>
发送:AT+MQTTCONFIG=183.230.40.16,8883,MQTT002,1,60,431671,version=2018-10-31&res=products%2F431671%2Fdevices%2FMQTT002&et=1635216369&method=md5&sign=UJyDU%2FI3%2FVGCJFjUHhBAXw%3D%3D
响    应:
OK
</pre>
### 查询命令
<pre>
发送:AT+MQTTCONFIG?
响    应:
+MQTTCONFIG:183.230.40.16,8883,MQTT002,1,60,431671,version=2018-10-31&res=products%2F431671%2Fdevices%2FMQTT002&et=1635216369&method=md5&sign=UJyDU%2FI3%2FVGCJFjUHhBAXw%3D%3D
OK
</pre>
## AT+MQTTCONNECT 客户端连接服务器
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+MQTTCONNECT</td><td>+MQTTCONNECT:0<br>OK<br>ERROR</td></tr></table>
MQTT CONNECT连接超时默认是5S。
### 执行命令
<pre>
发送命令:AT+MQTTCONNECT
响    应:
+MQTTCONNECT:0
OK
</pre>
## AT+MQTTPUB 发布消息
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+MQTTPUB=?</td><td>+MQTTPUB:&lt;topic&gt;,(0-2),(0-1);&lt;message&gt;</td></tr><tr><td  style="vertical-align: middle;">+MQTTPUB=&lt;topic&gt;,&lt;qos&gt;,&lt;retain&gt;&lt;message&gt;</td><td>OK<br>ERROR</td></tr></table>
&lt;topic&gt;:发布主题
&lt;qos&gt;:消息质量

+ 0:最多一次
+ 1:最少一次
+ 2:只做一次

&lt;message&gt;:用户消息
### 测试命令
<pre>
发送:AT+MQTTPUB=?
响    应:
+MQTTPUB:&lt;topic&gt;,(0-2),(0-1);&lt;message&gt;
OK
</pre>
### 设置命令
<pre>
发送:AT+MQTTPUB="$sys/431671/MQTT002/dp/post/json",1,0,"{\"id\":123,\"dp\":{\"temperatrue\":[{\"v\":35,}],\"power\":[{\"v\":61,}]}}"
响    应:
OK
</pre>

## AT+MQTTSUB 订阅主题
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+MQTTSUB=&lt;topic&gt;,&lt;qos&gt;</td><td>OK<br>ERROR</td></tr><td  style="vertical-align: middle;">+MQTTTLS?</td><td>+MQTTSUB:&lttopic&gt;,(0-2)<br>OK</td></tr></table>
&lt;topic&gt;:订阅主题<br>
&lt;qos&gt;:消息质量

+ 0:最多一次
+ 1:最少一次
+ 2:只做一次

## AT+MQTTUNSUB 取消订阅主题
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+MQTTUNSUB=&lt;topic&gt;</td><td>OK<br>ERROR</td></tr><tr><td  style="vertical-align: middle;">+MQTTCONFIG?</td><td>+MQTTUNSUB:&lt;topic&gt;<br>OK</td></tr></table>
&lt;topic&gt;:订阅主题
## AT+MQTTSTATE 查询连接状态
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+MQTTSTATE</td><td>+MQTTSTATE:&lt;status&gt;<br>OK<br>ERROR</td></tr></table>
&lt;status&gt;:状态

+ 0:连接服务器失败
+ 1:连接服务器成功

##  执行命令
<pre>
发送:AT+MQTTSTATE
响    应:
OK
</pre>
## AT+MQTTDISCONNECT 客户端断开连接
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+MQTTDISCONNECT</td><td>OK<br>ERROR</td></tr></table>
## MQTTS 实例
<pre>
AT+CFUN=1  //上电
OK

AT+MQTTTLS=1,0,"-----BEGIN CERTIFICATE-----......",1181  //设置TLS证书
OK

AT+MQTTCONFIG=183.230.40.16,8883,MQTT002,1,60,431671,verson  //设置MQTT参数
OK

AT+MQTTCONNECT  //连接MQTT服务器
+MQTTCONNECT:0
OK

AT+MQTTSTATE  //查询链接状态
+MQTTSTATE:1
OK

AT+MQTTPUB="$sys/431671/MQTT002/dp/post/json",1,0,"{\"id\":123}" //发布主题
OK

AT+MQTTDISCONNECT  //断开mqtt连接
OK



