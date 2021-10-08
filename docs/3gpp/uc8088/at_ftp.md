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



#  FTP指令
## AT+FTPMODE 设置FTP模式 
AT+FTPMODE支持主动模式和被动模式。

<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPMODE= &lt;value&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+FTPMODE?</td><td>+FTPMODE: &lt;value&gt;<br>OK</td></tr><tr><td>+FTPMODE=?</td><td>+ FTPMODE: (0-1)</td></tr></table>
&lt; value &gt;: 设置FTP模式值

+ 0:主动模式<br>
+ 1:被动模式

### 测试命令
<pre>发送命令：AT+FTPMODE=?
响    应：
+FTPMODE: （0-1）
OK

说明：
+FTPMODE: （0-1）
Mode值: 0/1 ，0代表主动模式，1代表被动模式。</pre>
### 设置命令
<pre>发送命令：AT+ FTPMODE =0
响    应：
OK

说明：
返回OK表示设置成功。</pre>
### 查询命令
<pre>发送命令：AT+ FTPMODE?
响    应：
+FTPMODE: 0
OK

说明:
+FTPMODE: 0
1: FTP工作在被动模式下
0：FTP工作在主动模式下</pre>
## AT+FTPTYPE 设置 FTP 数据传输类型 
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPTYPE= &lt;value&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+FTPTYPE?</td><td>+FTPTYPE: &lt;value&gt;<br>OK</td></tr><tr><td style="vertical-align: middle;">+FTPTYPE=?</td><td>+ FTPTYPE: (I/A)</td></tr></table>
FTPTYPE:(I/A)

+ I:FTP Binary 字符集
+ A:FTP ASCII 字符集

## AT+FTPRESET FTP 断点续传
AT+ FTPRESET  标识出文件内的数据点，将从这个点开始继续传送文件
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPRESET= &lt;value&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+FTPRESET?</td><td>+FTPRESET:&lt;value&gt;<br>OK</td></tr><tr><td>+FTPRESET=?</td><td>+ FTPRESET:&lt;value&gt;</td></tr></table>
### 2.28.1 测试命令
<pre>发送命令：AT+ FTPRESET =?
响    应：
+FTPRESET:&lt;value&gt;
OK

说明：
+FTPRESET:&lt;value&gt;
&lt;value&gt;:传输文件的偏移比特数</pre>
### 设置命令
<pre>发送命令：AT+ FTPRESET=5 
响    应：
OK

说明：
返回OK表示成功。
RESET命令只在TYPE i模式下有效</pre>
###<span id=2.28.1><font color=#2f5496>查询命令<font color=bla>
<pre>发送命令：AT+ FTPRESET?
响    应：
+FTPRESET:5
OK</pre>
## AT+FTPSERV 设置FTP服务地址
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPSERV= &lt;ip&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+FTPSERV?</td><td>+FTPSERV:&lt;ip&gt;<br>OK</td></tr><tr><td>+FTPSERV=?</td><td>+ FTPSERV:&lt;ip&gt;</td></tr></table>
结果返回码列表
<table><tr><td width =123456>返回码</td><td width =83%>含义</td></tr><tr><td>0</td><td>成功</td></tr><tr><td>1</td><td>pdp未激活</td></tr><tr><td>2</td><td>ip地址有误</td></tr><tr><td>3</td><td>网络ip连接不上</td></tr><tr><td>4</td>
<td>ip连接超时</td></tr><tr><td>5</td><td>服务器ftp拒绝</td></tr></table>
&lt;ip&gt;: 设置FTP服务器的IP地址
### 测试命令
<pre>发送命令：AT+ FTPSERV =?
响    应：
+FTPMODE: &lt;IP&gt;
OK

说明：
+FTPSERV: &lt;IP&gt;
IP值: IP地址</pre>
### 设置命令
<pre>发送命令：AT+ FTPSERV =129.168.0.1
响    应：+FTPSERV:0
OK

说明：
返回OK表示设置成功。</pre>
### 查询命令
<pre>发送命令：AT+ FTPSERV?
响    应：
+FTPSERV: 129.168.0.1
OK

说明:
+FTPSERV: 129.168.0.1
已经设置的FTP IP 地址</pre>
## AT+FTPUN 设置FTP用户名
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPUN= &lt;username&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+ FTPUN?</td><td>+ FTPUN: &lt;username&gt;<br>OK</td></tr><tr><td>+ FTPUN=?</td><td>+ FTPUN: &lt;username&gt;</td></tr></table>


&lt;username&gt;: 设置登录FTP客户端的用户名。
### 测试命令
<pre>发送命令：AT+ FTPUN =?
响    应：
+FTPUN: &lt;username&gt;
OK

说明：
+FTPUN: &lt;username&gt;
username值: FTP 客户端的用户名字，执行FTPUN时，登录FTP客户端。用户名最大长度为64字节。</pre>
### 设置命令
<pre>发送命令：AT+ FTPUN =xs
响    应：
OK

说明：
返回OK表示设置成功。</pre>
### 查询命令
<pre>发送命令：AT+ FTPUN?
响    应：
+FTPUN: xs
OK

说明:
+FTPUN: xs
已经设置的FTP客户端用户名</pre>
## AT+FTPPW 设置FTP密码
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPPW= &lt;password&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+ FTPPW?</td><td>+ FTPPW: &lt;password&gt;<br>OK</td></tr><tr><td style="vertical-align: middle;">+ FTPPW=?</td><td>+ FTPUN: &lt;password&gt;</td></tr></table>
&lt;password&gt;: 设置登录FTP客户端的密码，最长为64字节。
### 测试命令
<pre>发送命令：AT+ FTPPW =?
响    应：
+FTPPW: &lt;password&gt;
OK

说明：
+FTPPW: &lt;password&gt;
password值: FTP 客户端的用户密码，执行FTPPW时，登录FTP客户端用户名已经设置成功了。密码最大长度为64字节。</pre>
### 设置命令
<pre>发送命令：AT+ FTPPW =123
响    应：
OK

说明：
返回OK表示设置成功。</pre>
### 查询命令
<pre>发送命令：AT+ FTPPW?
响    应：
+FTPPW: 123
OK

说明:
+FTPPW: 123
已经设置的FTP客户端用户密码</pre>
## AT+FTPGETNAME 设置FTP客户端get文件名字
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+ FTPGETNAME = &lt;file name&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+ FTPGETNAME?</td><td>+ FTPGETNAME&lt;file name&gt;<br>OK</td></tr><tr><td>+ FTPGETNAME=?</td><td>+ FTPGETNAME: &lt;file name&gt;</td></tr></table>
&lt;file name &gt;: 设置FTP get方式获取文件的名字。文件名字最长长度为64字节。
### 测试命令<font color=bla>
<pre>发送命令：AT+ FTPGETNAME =?
响    应：
+FTPGETNAME:&lt;file name&gt;
OK

说明：
+FTPGETNAME: &lt;file name&gt;
file name值: FTP 客户端 get方式获取文件的名字</pre>
### 设置命令
<pre>FTP服务连接 
发送命令：AT+ FTPGETNAME =123.txt
响    应：
OK

说明：
返回OK表示设置成功。</pre>
### 查询命令
<pre>发送命令：AT+ FTPGETNAME?
响    应：
+FTPGETNAME:123.txt
OK

说明:
+FTPGETNAME: 123.txt
已经设置的FTP get方式获取的文件名字</pre>
## AT+FTPPATH FTP切换路径
AT+ FTPPATH设置FTP 获取文件路径。
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+ FTPPATH = &lt;file path&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+ FTPPATH?</td><td>+ FTPPATH：&lt;file path&gt;<br>OK</td></tr><tr><td>+ FTPPATH=?</td><td>+ FTPPATH: &lt;file path&gt;</td></tr></table>
&lt;file path&gt;: 设置FTP获取文件的路径。文件路径最长长度为64字节。
### 测试命令
<pre>发送命令：AT+ FTPPATH=?
响    应：
+FTPPATH:XXX
OK

说明：
+FTPPATH: < file path>
file path值: FTP 客户端获取文件的路径。</pre>
### 设置命令
<pre>FTP获取文件的路径 
发送命令：AT+ FTPPATH=/docs
响    应：
OK

说明：
返回OK表示设置成功。</pre>
### 查询命令
<pre>发送命令：AT+ FTPPATH?
响    应：
+FTPPATH: /docs
OK

说明:
+FTPPATH: /docs
已经设置的FTP获取的文件路径</pre>
## AT+FTPPUTNAME 设置FTP客户端put的文件名字
AT+ FTPPUTNAME设置FTP put文件名字。
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+ FTPPUTNAME = &lt;put name&gt;</td><td>OK<br>ERROR</td></tr><tr><td style="vertical-align: middle;">+ FTPPUTNAME?</td><td>+FTPPUTNAME:&lt;put name&gt;<br>OK</td></tr><tr><td>+ FTPPUTNAME=?</td><td>+ FTPPUTNAME: &lt;put name&gt;</td></tr></table>
&lt;put name &gt;: 设置FTP put方式获取文件的名字。文件名字最长长度为64字节。
### 测试命令
<pre>发送命令：AT+ FTPPUTNAME =?
响    应：
+FTPPUTNAME:&lt;put name&gt;
OK

说明：
+FTPPUTNAME: &lt; put name &gt;
put name值: FTP 客户端 put方式获取文件的名字。</pre>
### 设置命令
<pre>发送命令：AT+ FTPPUTNAME =123.txt
响    应：
OK

说明：
返回OK表示设置成功。</pre>
###查询命令
<pre>发送命令：AT+ FTPPUTNAME?
响    应：
+FTPPUTNAME: 123.txt
OK

说明:
+FTPPUTNAME: 123.txt
已经设置的FTP put方式获取的文件名字</pre>
## AT+FTPMKD FTP创建目录
AT+ FTPMKD创建目录
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+ FTPMKD = &lt;dir&gt;</td><td>OK<br>ERROR</td></tr><tr><td>+ FTPMKD=?</td><td>+ FTPMKD: &lt;dir&gt;</td></tr></table>
&lt;dir &gt;: FTP创建目录,目录最长长度为64字节。
### 测试命令
<pre>发送命令：AT+ FTPMKD =?
响    应：
+FTPMKD:&lt;dir&gt;
OK

说明：
+FTPMKD: &lt; dir &gt;
dir值: 文件目录</pre>
### 设置命令
<pre>发送命令：AT+ FTPMKD =/invoker
响    应：
OK

说明：
返回OK表示设置成功。</pre>
## AT+FTPRMD FTP删除目录
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+ FTPRMD = &lt;dir&gt;</td><td>OK<br>ERROR</td></tr><tr><td>+ FTPRMD=?</td><td>+ FTPMKD: &lt;dir&gt;</td></tr></table>
&lt;dir &gt;: FTP删除目录,目录最长长度为64字节。
###<font color=#2f5496>测试命令<font color=bla>
<pre>发送命令：AT+ FTPRMD =?
响    应：
+FTPRMD:&lt;dir&gt;
OK

说明：
+FTPRMD: &lt; dir &gt;
dir值: 文件目录</pre>
### 设置命令
<pre>发送命令：AT+ FTPRMD =/invoker
响    应：
OK

说明：
返回OK表示设置成功。</pre>
## AT+FTPGET FTP GET文件
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+ FTPGET</td><td>+FTPGET:lent<br>&lt;data&gt;<br>OK<br>ERROR</td></tr><tr><td>+ FTPGET=?</td><td>+FTPGET</td></tr></table>
### 测试命令
<pre>发送命令：AT+ FTPGET?
响    应：
+FTPGET
OK
</pre>
### 执行命令
<pre>
发送命令：AT+ FTPGET 
响    应：
OK

说明：
返回OK表示设置成功。
</pre>
## AT+FTPPUT FTP PUT文件
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+ FTPPUT=lent,&lt;data&gt;</td><td>OK<br>ERROR</td></tr><tr><td>+ FTPPUT=?</td><td>+FTPPUT</td></tr></table>
FTP PUT 文件内容。 PUT文件完成后，以”AT+ FTPPUT=0,”结尾。否则FTP一直处于上传状态。
### 测试命令
<pre>发送命令：AT+ FTPPUT=?
响    应：
+FTPPUT
OK</pre>
### 执行命令
<pre>发送命令：AT+ FTPPUT=2,12
响    应：
OK

说明：
发送内容为”12”, 长度为2. 返回OK表示设置成功。</pre>
## AT+FTPLIST FTP文件目录列表
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPLIST</td><td>+FTPLIST:mode,lent<br>&lt;file list&gt;<br>OK<br>ERROR</td></tr></table>

### 执行命令
<pre>FTP服务文件目录列表 
发送命令：AT+ FTPLIST
响    应：
+FTPLIST:1,327
-rw-rw-r--  1  xisen xisen 5 jul 07 09:30 123.txt
......
OK

说明：
返回OK表示设置成功。</pre>
## AT+FTPSCON 查询FTP配置
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPSCON</td><td>+FTPSCON<br>XXXX<br>OK<br>ERROR</td></tr><tr><td>+ FTPSCON=?</td><td>+FTPSCON</td></tr></table>
### 测试命令
<pre>获取写命令使用方法
发送命令：AT+FTPSCON =?
响    应：
+FTPSCON
OK</pre>
### 查询命令
<pre>发送命令：AT+FTPSCON 
响    应：
+FTPSCON ：
XXX
OK

说明：
返回配置信息。</pre>
## AT+FTPQUIT退出FTP
<table><tr><td width=12345>Command</td><td width =60%>Possible response(s)</td></tr><tr><td style="vertical-align: middle;">+FTPQUIT</td><td>OK<br>ERROR</td></tr><tr><td>+FTPQUIT=?</td><td>+FTPQUIT</td></tr></table>
### 测试命令

<pre>发送命令：AT+ FTPQUIT =?
响    应：
+FTPQUIT
OK</pre>
### 执行命令
<pre>发送命令：AT+ FTPQUIT 
响    应：
OK
说明：退出FTP成功</pre>
## FTP实例

<pre>

AT+CFUN=1  //上电
OK

AT+CGACT=1,1  //PDP激活
OK

AT+FTPPORT=15881  //设置ftp服务器端口号
OK

AT+FTPMODE=1  //设置被动传输模式
OK

AT+FTPSERV=47.104.157.10  //连接FTP服务器
+FTPSERV:0
OK

AT+FTPUN=xs  //设置用户名
OK

AT+FTPPW=123  //设置密码
OK

AT+FTPTYPE=I 设置数据传输类型
OK

AT+FTPPUTNAME=12.txt  //设置put文件名,准备上传数据
OK

AT+FTPPUT =5,12345  //传输数据"12345"至所设置的put文件
OK

AT+FTPPUT=0  //停止传输数据
OK

AT+FTPGETNAME=12.txt //设置get文件名
OK

AT+FTPRESET=2  //设置断点
OK

AT+FTPGET  //接收数据
+FTPGET:3,345
OK

AT+FTPLIST //读取文件目录列表
+FTPLIST:1,48
-rw-rw-r--  1 xisen xisen  5 Sep 30 01:52 12.txt
OK

AT+FTPQUIT  //断开FTP链接
OK

</pre>