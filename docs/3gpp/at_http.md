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
# HTTP指令
## AT+HTTPTLS 设置HTTPS的TLS参数
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+HTTPTLS=&lt;tls&gt;</td><td>OK<br>ERROR</td></tr><tr><td  style="vertical-align: middle;">+HTTPTLS?</td><td>+HTTPTLS:0<br>OK</td></tr><tr><td  style="vertical-align: middle;">+HTTPTLS=?</td><td>+HTTPTLS:(0-1)<br>OK</td></tr></table>
&lt;tls&gt; tls使能

+ 0: 非tls
+ 1: tls

### 测试命令
<pre>发送命令：AT+HTTPTLS=?
响    应：
+HTTPTLS:(0-1)
OK</pre>
### 设置命令
<pre>发送命令：AT+HTTPTLS=1
响    应：
OK
说明：使能HTTPS</pre>
### 查询命令
<pre>查询HTTPS的TLS参数
发送命令：AT+HTTPTLS?
响    应：
+HTTPTLS:1
OK</pre>
##<font color=#2f5496>AT+HTTPPARA设HTTP参数<font color=bla>
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+HTTPPARA=[&lt;httpparatag&gt;:&lt;httpparavalue&gt;]</td><td>OK<br>ERROR</td></tr><tr><td>+HTTPPARA?</td><td></td></tr><tr><td>+HTTPPARA=?</td><td></td></tr></table>
该命令用于HTTP 参数配置&lt;httpparavalue&gt; 参数说明：根据&lt;httpparatag&gt;参数类型给数据。
&lt;httpparatag&gt;参数说明:<br>
&lt;URL&gt;：必选参数，客户端 URL（http:\\xxxx 或者 192.168.1.1），最长参数1024字节；<br>
&lt;PORT&gt;: HTTP代理服务器的端口；<br>
&lt;TIMEOUT&gt;:HTTP超时时间，默认时间50S；<br>
&lt;USRE&gt;:HTTP user数据，最大长度512字节，（数据中“\r\n”需要转义）同AT+HTTPDATA;<br>
&lt;BODY&gt;:HTTP body参数，最大长度512字节；<br>
&lt;MODE&gt;:HTTP 数据主动读模式和数据上报模式。<br>
### 测试命令
<pre>
发送命令：AT+HTTPPARA =?
响    应：
+HTTPPARA: [&lt;httpparatag&gt;:&lt;httparavalue&gt;]
OK

说明：
+HTTPPARA: [&lt;httpparatag&gt;:&lt;httparavalue&gt;]
httpparatag: 
httparavalue:
</pre>
### 设置命令
<pre>发送命令：AT+HTTPPARA =xx,xxx
响    应：
OK
说明：设置参数成功</pre>
### 查询命令
<pre>发送命令：AT+HTTPPARA?</pre>
## AT+HTTPDATA HTTP数据
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+HTTPDATA=[&lt;lent&gt;,&lt;data&gt;]</td><td>OK<br>ERROR</td></tr><tr><td>+HTTPDATA=?</td><td>+HTTPDATA:&lt;lent&gt;,&lt;data&gt;</td></tr></table>
### 测试命令
<pre>发送命令：AT+HTTPDATA=?
响    应：
+HTTPDATA: &lt;lent&gt;,&lt;data&gt;
OK

说明：
+HTTPDATA: &lt;lent&gt;,&lt;data&gt;
&lt;lent&gt;: http数据的长度
&lt;data&gt;:http数据</pre>
### 设置命令
<pre>发送命令：AT+HTTPDATA=1,X
响    应：
OK
说明：设置写数据成功</pre>
## AT+HTTPACTION HTTP激活模式
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td>+HTTPACTION=(0-3)</td><td>+HTTPACTION=&lt;STATE&gt;,&lt;LENT&gt;<br>OK<br>ERROR</td></tr><tr><td>+HTTPACTION=?</td><td>+HTTPACTION:(0-3)</td></tr></table>
HTTP 激活。激活模式：

+ 0：get方式
+ 1：head方式
+ 2：put方式
+ 3：post方式

&lt;STATE&gt; 返回HTTP请求类型；<br>
&lt;LENT&gt; 返回HTTP后有效数据的长度
### 测试命令
<pre>查看HTTP 激活模式参数范围
发送命令：AT+HTTPACTION=?
响    应：
+CGDCONT: (0-3)
OK</pre>
### 设置命令
<pre>发送命令：AT+HTTPACTION=0
响    应：
+HTTPACTION=0，192
OK
说明：激活成功。</pre>
## AT+HTTPREAD读取http响应数据
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+HTTPREAD</td><td>+HTTPREAD=&lt;lent&gt;,&lt;data&gt;<br>OK<br>ERROR</td></tr><tr><td>+HTTPREAD=?</td><td>+HTTPREAD</td></tr></table>
### 测试命令
<pre>发送命令：AT+HTTPREAD=?
响    应：
+HTTPREAD
OK</pre>
### 执行命令
<pre>发送命令：AT+HTTPREAD
响    应：
+HTTPREAD:XXX,
XXX
OK
说明：HTTP 读取响应数据</pre>
## AT+HTTPTERM 关闭HTTP
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td   style="vertical-align: middle;">+HTTPREAD</td><td>OK<br>ERROR</td></tr></table>
### 执行命令
<pre>发送命令：AT+ HTTPTERM 
响    应：
OK
说明：关闭HTTP成功</pre>


##HTTP实例
<pre>
AT+CFUN=1 //上电
OK

AT+CGACT=1,1  //pdp激活
OK

AT+HTTPPARA=user,api-key:vRa2kc6wXGtx6BHA5aAga0gmtiA=  
OK

AT+HTTPARA=url,http://api.heclouds.com/devices/729083795/datapoints
OK

AT+HTTPINIT
OK

AT+HTTPPARA=body,{'datastreams':['id':'humi','datapoints':[{'value':92}]],{'id':'temperature','datapoints':[{'value':29}]}}
OK

AT+HTTPACTION=3
+HTTPACTION:0,26
OK

AT+HTTPTERM
OK
</pre>

