# 设备参数静态信息

| 名称             | 含义       | 长度 | 默认值           | 备注     |
| ---------------- | ---------- | ---- | ---------------- | -------- |
| imei             | 注册的IMEI | 16   | 1234567890123456 | 长度固定 |
| dev_name         | 设备的名字 | 16   | ucchip           | 长度可变 |
| dev_serial       | 设备串口号 | 16   | 123456           | 长度可变 |
| soft_ver         | 软件版本号 | 16   | v1.0             | 长度可变 |
| manufacture_name | 厂商名字   | 16   | ucchip8088       | 长度可变 |
| hardware_ver     | 硬件版本号 | 16   | v1.0             | 待处理   |
| N/A              | 预留       | 64   | 0                | 长度可变 |

# 设备控制类静态信息

<table>
	<tr>
	    <th>名称</th><th>含义</th><th>长度</th><th>默认值</th><th>备注</th>
	</tr >
	<tr >
        <td style="vertical-align:middle;">op_modem</td><td style="vertical-align:middle;">设备运行模式</td><td style="vertical-align:middle;">4</td><td style="vertical-align:middle;">0</td><td>0：单模GPRS;<br>1：单模GPS;<br>2：双模，默认启动GPRS;<br>3：双模，默认启动GPS;</td>
	</tr>
	<tr>
	    <td style="vertical-align:middle;">trace_mode</td><td>Trace工具启动模式</td><td>4</td><td>1</td><td>1：START模式<br>2：WAIT模式</td>
	</tr>
	<tr style="vertical-align:middle;" >
        <td>reboot_time</td><td>设备重启控制</td><td>4</td><td>0</td><td>0：不启动功能<br>> 0 (单位：min)：启动功能</td>
	</tr>
	<tr>
	<td style="vertical-align:middle;">net search mode</td><td style="vertical-align:middle;">搜网模式</td><td style="vertical-align:middle;">4</td><td style="vertical-align:middle;">0</td><td>0：默认值(0xFF)，搜网间隔3S，10S，10S，10S，20S，20S，20S，20S，20S，20S，20S，20S，20S，20S，20S，60S，60S，60S，60S，60S，120S，360S...<br>1：搜网间隔3S...<br>2：搜网间隔3S、10S...<br>3：搜网间隔3S，10S，10S，10S，20S...</td>
	</tr>
	<tr style="vertical-align:middle;" >
        <td>N/A</td><td>预留</td><td>52</td><td>0</td><td>-</td>
    </tr>
</table>


# DFE静态信息

| 地址 | 含义                | 长度 | 默认值 | 备注                        |
| ---- | ------------------- | ---- | ------ | --------------------------- |
| 0    | phase step          | 1    | -      | CSR: 0x68                   |
| 1    | phase ctrl          | 1    | -      | CSR: 0x6c                   |
| 2    | Tx 2nd level Coef 0 | 1    | -      | CSR: 0x70                   |
| 3    | Tx 2nd level Coef 1 | 1    | -      | CSR:0x74                    |
| 4    | Tx 2nd level Coef 2 | 1    | -      | CSR: 0x78                   |
| 5    | Tx 2nd level Coef 3 | 1    | -      | CSR: 0x7c                   |
| 6    | Tx 2nd level Coef 4 | 1    | -      | CSR: 0x80                   |
| 7    | filter ctrl 2       | 1    | -      | CSR: 0x98                   |
| 8    | tx 1st level coef   | 16   | -      | 0x200 ~ 0x200 + 15DW        |
| 24   | RX AGC Comparision  | 16   | -      | 0x200 + 48DW~ 0x200 + 63DW  |
| 40   | AGC LUT             | 32   | -      | 0x200 + 96DW~ 0x200 + 127DW |


# RF校准静态信息

| 信息                 | 含义                         | 长度 | 默认值 | 备注   |
| -------------------- | ---------------------------- | ---- | ------ | ------ |
| po_vramp_lut         | -                            | 120  | -      | 2*20*3 |
| temp_list            | -                            | 64   | -      | 1*64   |
| tem_freq_offset_list | -                            | 256  | -      | 4*64   |
| temp_freq_delta      | -                            | 4    | -      | -      |
| adc_digi_pwr_bit     | ADC cali                     | 1    | -      | -      |
| adc_cal_refi_bit     | -                            | 1    | -      | -      |
| ramp_dac_gcal        | -                            | 1    | -      | -      |
| ramp_dac_off         | -                            | 1    | -      | -      |
| dcxo_afc_bits        | Init witch calibration value | 2    | -      | -      |
| afc_gain             | -                            | 2    | -      | -      |
| dcxo_coarse_cdac     | -                            | 1    | -      | -      |
| vicp_ratio           | -                            | 1    | -      | -      |
| vbg_cali             | -                            | 1    | -      | -      |
| rcali                | -                            | 1    | -      | -      |

