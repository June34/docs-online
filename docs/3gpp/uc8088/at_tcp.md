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
#  TCP指令
## AT+IPCREATE 创建Socket
AT+IPCREATE 支持创建TCP和UDP客户端。当前只能支持4个客户端，不支持服务端。
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+IPCREATE= [&lt;index&gt;],[&lt;modem&gt;],[&lt;adress&gt;],[&lt;Port&gt;]</td><td>OK<br>&lt;index&gt;,CONNECT OK<br>CONNECT FAIL<br>ERROR</td></tr><tr><td>+IPCREATE?</td><td>+IPCREATE: &lt;index&gt;,&lt;state&gt;</td></tr><tr><td>+IPCREATE=?</td><td>+IPCREATE:index,mode(TCP/UDP),ip/address,port</td></tr></table>
创建TCP或者UDP链接。如果创建网络链接前PDP没有激活，将自动完成PDP激活后创建。如果创建TCP连接连续3次都失败了，将返回“CONNECT FAIL”；TCP创建成功返回“CONNECT OK”

&lt;index&gt;: 创建连接的通道，目前可选0-3四个通道<br>
&lt;modem&gt;: 创建TCP连接或者UDP连接<br>
&lt;adress&gt;: 创建客户端要连接服务端的地址，可以用IP地址或者域名，具有将域名转换成IP的功能。<br>
&lt;Port&gt;:服务器的端口号<br>
### 测试命令
<pre>发送命令：AT+IPCREATE=?
响    应：
+IPCREATE: index,mode(TCP/UDP),ip/address,port
OK

说明：
+IPCREATE: index,mode(TCP/UDP),ip/address,port
Index:数字0-3
mode(TCP/UDP): TCP或者UDP字符串
ip/address: 地址
Port:端口</pre>

### 设置命令
<pre>发送命令：AT+IPCREATE=0，”TCP”,”192.168.1.1”,50
响    应：
OK
CONNECT OK
说明：
返回OK表示创建socket指令已经执行。
返回CONNECT OK说明创建TCP连接成功。</pre>

### 查询命令
<pre>发送命令：AT+ IPCREATE?
响    应：
+IPCREATE: 
0,TCP
1,NUL
2.NUL
3.NUL
OK

说明:
+IPCREATE: 
0,TCP
1,NUL
2.NUL
3.NUL
OK
0:通道号
TCP：通道状态</pre>
## AT+IPSEND Socket发送数据
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+IPSEND = [&lt;index&gt;],[&lt;lent&gt;],[&lt;data&gt;]</td><td>+IPSEND =lent<br>OK<br>ERROR</td></tr><tr><td>+IPSEND =?</td><td>+IPSEND :index,lent,data</td></tr></table>
通过已经建立的SOCKET发送数据。
&lt;index&gt;：已创建连接的通道
&lt;lent&gt;：发送的数据的长度
&lt;data&gt;：发送的数据
### 测试命令
<pre>发送命令：AT+IPSEND =?
响    应：
+IPCREATE: index,lent,data
OK

说明：
+IPCREATE: index,lent,data
index: 已创建连接的通道
lent: 发送数据长度
data:数据
</pre>
### 设置命令
<pre>发送命令：AT+IPSEND=0,3,”123”
响    应：
+IPSEND:3
OK

说明：
返回OK表示发送数据已经执行完成。
返回+IPCREATE=3说明成功发送数据的长度。</pre>
## AT+IPCLOSE 关闭Socket
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td  style="vertical-align: middle;">+IPCLOSE = [&lt;index&gt;]</td><td>OK<br>ERROR</td></tr><tr><td>+IPCLOSE =?</td><td>+IPCLOSE :index</td></tr></table>
关闭已经创建的Socket. 重启Socket，必须已经存在的Socket才能重新创建。
&lt;index&gt;： 关闭通道号<br>
### 测试命令
<pre>发送命令：AT+IPCLOSE =?
响    应：
+IPCLOSE :index
OK

说明：
+IPCLOSE :index
OK
index: 关闭通道号</pre>
### 设置命令
<pre>发送命令：AT+IPCLOSE =0
响    应：
OK

说明：
返回OK表示socket关闭成功。</pre>
## 完整TCP数据传输流程
<pre>
AT+CFUN=1  //设备上电
OK

AT+CGACT=1,1  //pdp激活
OK

AT+IPCREATE? //查询是否已经存在SOCKET
+IPCREATE:
0,NUL
1,NUL
2,NUL
3,NUL
OK

AT+IPCREATE=0,TCP,183.230.40.40,1811   //创建TCP
0,CONNECT OK
OK

AT+IPSEND=0,3,"123" //TCP发送"123"给服务器
+IPSEND:3
OK

+IPRECV:0,"TCP",8,received  //TCP接收上报TCP数据

AT+IPCLOSE=0 //关闭TCP连接
0,TCPCLOSE
OK
</pre>
