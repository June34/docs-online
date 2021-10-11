# <span id=1><font color=#2f5496>文档介绍  </span>
##<span id=1.1><font color=#2f5496>文档范围
<font color=bla>本手册详细介绍了UC8088 GPRS模块提供的AT指令集。
##<span id=1.2><font color=#2f5496>惯例和术语缩写
<font color=bla>在本手册中可能会用到的术语解释如下：
+ MT	移动终端
+ TA	终端适配器 
+ TE 终端设备
+ SIM 用户识别模块
+ ME	移动设备，包括MT，TA和TE等功能部件
+ MS	移动台，包括ME和SIM
+ DCE 数据通信设备，或者传真DCE（传真调制解调器，传真板）
+ DTE 数据终端设备，即一个嵌入式应用

在实际应用中，GPRS模块可能被称为ME，MS，TA或DCE，而通过串口发AT指令来控制GPRS模块的控制器则可能被称为TE或DTE。
##<span id=1.3><font color=#2f5496>命令语法
###<span id=1.3.1><font color=#2f5496>命令格式
<font color=bla>本手册中所有命令行必须以”AT”或”at”作为开头，以回车（&lt;CR&gt;）作为结尾。响应通常紧随命令之后，且通常以”<回车><换行><响应内容><回车><换行>”（&lt;CR&gt;&lt;LF&gt;<响应内容>&lt;CR&gt;&lt;LF&gt;）的形式出现。在命令介绍时，“<回车><换行>”（&lt;CR&gt;&lt;LF&gt;）通常被省略了。
###<span id=1.3.2><font color=#2f5496>命令类型
<font color=bla>通常命令可以有如下表所示的四种类型中的一种或多种形式。
<table><tr><th width=20%>类型</th><th width=30%>格式</th><th width=99999>说明</th></tr><tr><td> 测试命令 </td><td>AT+&lt;cmd&gt;=?</td><td> 用于查询设置命令或内部程序设置的参数及其取值范围</td>  </tr>  <tr>    <td>查询命令 </td>    <td>AT+&lt;cmd&gt;?</td>    <td>用于返回参数的当前值</td>  </tr>  <tr>    <td>设置命令 </td>    <td>AT+&lt;cmd&gt;=<...></td>    <td>用于设置用户自定义的参数值</td>  </tr>  <tr>    <td>执行命令</td>    <td>AT+&lt;cmd&gt;</td>    <td>用于读取只读参数或不需要额外参数的情况</td>  </tr></table>
###<span id=1.3.3><font color=#2f5496>参数类型
<font color=bla>命令参数虽然多种多样，但是都可以简单地归结为整数类型和字符串类型（包括不带双引号的字符串和带双引号的字符串）这两种基本的类型，如下表所示。
<table><tr ><td width=99999>类型</td><td width=50%>示例</td></tr><tr><td>整数类型</td><td>123</td></tr><tr><td rowspan="2">字符串类型</td><td>abc</td></tr><tr><td>"hellow ,world"</td></tr></table><center>表2 参数类型</center>
##<span id=1.3><font color=#2f5496>注意事项
<font color=bla>
+ AT串口输入时不支持回删键(backspace)功能
+ 本文档+ERROR指+CME ERROR或者+EXT ERROR