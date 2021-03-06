## 控制命令
### 配置参数复位命令
控制当前设备恢复最初的各种配置参数（可通过配置命令设置的内容）。

| 命令         | 反馈信息                     | 说明                   | 备注                       |
| ------------ | ---------------------------- | ---------------------- | -------------------------- |
| $CFG,RST\r\n | #response#CFG,RST,ok!*12\r\n | 恢复设备最初的配置参数 | 只针对配置命令可设置的内容 |

### 冷启动命令
控制当前设备以冷启动的方式复位，无任何先验信息开始运行。

| 命令          | 反馈信息                      | 说明               | 备注       |
| ------------- | ----------------------------- | ------------------ | ---------- |
| $RST,COLD\r\n | #response#RST,COLD,ok!*54\r\n | 冷启动方式复位设备 | 无可用信息 |

### 温启动命令

控制当前设备以温启动的方式复位，以概略位置、时间信息和历书信息为先验信息，开始工作。

| 命令          | 反馈信息                      | 说明               | 备注                             |
| ------------- | ----------------------------- | ------------------ | -------------------------------- |
| $RST,WARM\r\n | #response#RST,WARM,ok!*59\r\n | 温启动方式复位设备 | 概略位置、时间信息和历书信息可用 |

### 热启动命令
控制当前设备以热启动的方式复位，以概略位置、时间信息、历书信息和星历信息为先验信息，开始运行。

| 命令         | 反馈信息                     | 说明               | 备注                                       |
| ------------ | ---------------------------- | ------------------ | ------------------------------------------ |
| $RST,HOT\r\n | #response#RST,HOT,ok!*03\r\n | 热启动方式复位设备 | 概略位置、时间信息、历书信息和星历信息可用 |
