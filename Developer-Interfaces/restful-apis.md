# RESTful API

## 一、提交测试请求
你可以通过执行POST或GET将测试提交到WebPagetest：http://www.webpagetest.org/runtest.php  
响应后，将被重定向到结果页面，或者获得一个xml响应（如果请求了xml）。
### 1.1 参数
| 参数名        | Required  | 描述 | 默认值 |
| :------------|---------------|:-------------|----------|
| url          | 必选 | 要测试的URL | |
| label        |     | 测试的标签   | |
| location     |     | 测试地点     | Dulles 5Mbps Cable |
| runs         |     | 测试运行次数（公共实例上为1-10） | 1 |
| fvonly       |     | 设置为1可跳过重复视图测试   | 0 |
| domelement   |     | DOM用于记录子测量的元素   |  |
| private      |     | 设置为1时，会隐藏测试日志   | 0 |
| connections  |     | 覆盖IE使用的并发连接数（0表示不覆盖）   | 0 |
| web10        |     | 设置为1时，测试在文档完全停止（onload）   | 0 |
| script       |     | 脚本测试执行   |  |
| block        |     | 空格分隔的URL列表（子字符串匹配）   |  |
| login        |     | 用于验证测试的用户名（http验证）   |  |
| password     |     | 用于验证测试的密码（http验证）   |  |
| authType     |     | 使用的认证类型：0 =基本认证，1 = SNS   | 0 |
| video        |     | 设置为1捕获视频（捕获视频用于计算`Speed Index`）   | 0 |
| f            |     | 格式。设置为“xml”以请求XML响应，而不是重定向，或者请求JSON编码响应的“json”   |  |
| r            |     | 当使用xml接口时，将在响应中打印   |  |
| notify       |     | 通过电子邮件地址，通知测试结果   |  |
| pingback     |     | 测试完成时ping的URL（测试ID将作为“id”参数传递）   |  |
| bwDown       |     | 下载带宽（以Kbps为单位）（在指定自定义连接配置文件时使用）   |  |
| bwUp         |     | 上传带宽（Kbps）（指定自定义连接配置文件时使用）   |  |
| latency      |     | 第一跳往返时间（以毫秒为单位）（指定自定义连接配置文件时使用）   |  |
| plr          |     | 丢包率 - 要丢弃的数据包的百分比（在指定自定义连接配置文件时使用）   |  
| k            |部分必选| API密钥（如果已分配） - 仅适用于`runtest.php`调用。 如果需要，请与网站所有者联系以获取密钥（http://www.webpagetest.org/getkey.php用于公共实例）|  |
| tcpdump      |     | 设置为1以启用tcpdump捕获   | 0 |
| noopt        |     | 设置为1以禁用优化检查（用于更快的测试）   | 0 |
| noimages     |     | 设置为1以禁用屏幕截图捕获   | 0 |
| noheaders    |     | 设置为1以禁用保存http头（以及浏览器状态消息和CPU利用率）   | 0 |
| pngss        |     | 设置为1可将完全加载的屏幕截图的全分辨率版本保存为png   |  |
| iq           |     | 为屏幕截图和视频捕获指定jpeg压缩级别（30-100）   |  |
| noscript     |     | 设置为1以禁用JavaScript（IE，Chrome，Firefox）   |  |
| clearcerts   |     | 设置为1以清除操作系统证书缓存（如果证书尚未缓存，则导致IE在SSL协商期间执行OCSP / CRL检查）。 于2.11加入   | 0 |
| mobile       |     | 设置为1可让Chrome模拟移动浏览器（屏幕分辨率，UA字符串，固定视口）。 于2.11加入   | 0 |
| keepua       |     | 设置为1以保留原始浏览器用户代理字符串（不要向其附加PTST）   |  |
| uastring     |     | 要使用的定制用户代理字符串   |  |
| width        |     | 视口（Viewport）宽度（以css像素为单位）   |  |
| height       |     | 视口（Viewport）高度（以css像素为单位）   |  |
|browser_width |     | 浏览器窗口宽度（以显示像素为单位）   |  |
|browser_height|     | 浏览器窗口高度（以显示像素为单位）   |  |
| dpr          |     | 模拟移动设备时使用的设备像素比率   |  |
| mv           |     | 在捕获视频时设置为1，以便只存储来自中值运行的视频。   | 0 |
| medianMetric |     | 计算中值运行时使用的默认指标   | loadTime |
| cmdline      |     | 自定义命令行选项（仅限Chrome）   |  |
| htmlbody     |     | 设置为1以保存第一个响应（基页）的内容，而不是所有文本响应（bodies= 1）   |  |
| tsview_id    |     | 将结果提交到tsviewdb时使用的测试名称（tsviewdb集成的私有实例）   |  |
| custom       |     | 在测试结束时收集的自定义指标   |  |
| tester       |     | 指定测试应运行的特定测试程序（必须与`/getTesters.php`中的PC名称匹配）。如果测试不可用，作业将永远不会运行。   |  |
| affinity     |     | 将测试哈希到特定测试代理的字符串。测试人员将根据可用测试人员的指数进行选择。如果测试器的数量改变，则测试将被分发到不同的机器，但是如果计数保持一致，则相同的字符串将总是在同一测试机器上运行测试。这可以用于在比较给定的URL随时间或不同的参数（使用URL作为哈希字符串）时控制可变性。   |  |
| timeline     |     | 设置为1可让Chrome捕获Dev Tools时间轴   | 0 |
|timelineStack |     | 设置为1到5之间，以使Chrome包含JavaScript调用堆栈。必须与“时间轴”结合使用。   | 0 |
| ignoreSSL    |     | 设置为1以忽略SSL证书错误，例如 名称不匹配，自签名证书等。   | 0 |
| mobileDevice |     | 来自mobile_devices.ini的设备名称，用于移动模拟（仅当指定mobile = 1才能启用模拟功能且仅适用于Chrome）   |  |
| appendua     |     | 要附加到用户代理字符串的字符串。 这是默认的`PTST/ver`字符串之外的。如果还指定了“keepua”，它将仍然附加。允许替换一些测试参数：  %TESTID% - 替换当前测试的测试ID  %RUN% - 用当前运行编号替换  %CACHED% - 用1代替重复视图测试，用0代替初始视图  %VERSION% - 使用当前wptdriver版本号替换|  |

### 1.2 指定连接
### 1.3 XML响应
### 1.4 Sample

## 二、检查测试状态
## 三、获取测试结果
### 3.1 Sample
## 四、取消测试
## 五、地点信息
