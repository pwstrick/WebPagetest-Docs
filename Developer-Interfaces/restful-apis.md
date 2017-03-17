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
| appendua     |     | 要附加到用户代理字符串的字符串。 这是默认的`PTST/ver`字符串之外的。如果还指定了“keepua”，它将仍然附加。允许替换一些测试参数：<br>%TESTID% - 替换当前测试的测试ID<br>%RUN% - 用当前运行编号替换<br>%CACHED% - 用1代替重复视图测试，用0代替初始视图<br>%VERSION% - 使用当前wptdriver版本号替换|  |

### 1.2 指定连接
如果未指定连接，则默认情况下将获取电缆（5/1 Mbps，28ms RTT）配置文件。格式如下：
```bash
location:browser.connectivity
```

示例：
```bash
Dulles_IE7.DSL
Frankfurt.Dial
China.custom
Dulles:Chrome.DSL
```
公共实例支持的配置文件有：
+ DSL - 1.5 Mbps下行，384 Kbps上行，50 ms第一跳RTT，0％分组丢失
+ Cable - 5 Mbps下行，1 Mbps上行，28ms第一跳RTT，0％丢包
+ FIOS - 20 Mbps下行，5 Mbps上行，4 ms第一跳RTT，0％丢包（不是所有位置都将获得全带宽）
+ Dial - 49 Kbps下行，30 Kbps上行，120 ms第一跳RTT，0％分组丢失
+ 3G - 1.6 Mbps下行，768 Kbps上行，300 ms第一跳RTT，0％丢包
+ 3GFast - 1.6 Mbps下行，768 Kbps上行，150 ms第一跳RTT，0％丢包
+ Native - No synthetic traffic shaping applied
+ custom - 自定义配置文件，带宽和延迟必须使用指定的bwIn，bwOut，latency和plr参数

浏览器只需在Chrome、Firefox安装，在多个浏览器中配置wptdriver。

### 1.3 XML响应
XML响应遵循REST API的格式。你将获得一个200的HTTP响应，结果是一个XML格式的信息。有关完整示例XML响应的示例，请参阅示例。
```xml
<response>
    <statusCode></statusCode>
    <statusText></statusText>
    <requestId></requestId>
    <data>
        <testId></testId>
        <xmlUrl></xmlUrl>
        <userUrl></userUrl>
        <summaryCSV></summaryCSV>
        <detailCSV></detailCSV>
    </data>
</response>
```
+ statusCode - 200表示成功提交。任何其他的都是一个错误（并将回到400与描述性文本）
+ statusText - 故障的错误信息
+ requestId - 请求ID来自于前面的`r参数`。如果未指定，将不存在。requestId使跟踪异步请求更容易。
+ testId - 分配给测试请求的ID（在所有网址中使用）
+ xmlUrl - 用于以XML格式获取结果的URL
+ userUrl - 将用户定向到结果页面的网址（如果不使用XML界面，通常会被重定向到的网址）
+ summaryCSV - 以CSV格式（页面级数据和时间）的摘要结果的网址。 如果测试尚未完成，将返回404。
+ detailCSV - URL格式的完整详细结果（请求级数据和时间）。 如果测试尚未完成，将返回404。

### 1.4 Sample
测试`www.aol.com`并重定向到结果页面：
```javascript
http://www.webpagetest.org/runtest.php?url=www.aol.com
```

测试`www.aol.com` 10次，首先查看并重定向到结果页面：
```javascript
http://www.webpagetest.org/runtest.php?url=www.aol.com&runs=10&fvonly=1
```

测试`www.aol.com` 2次，得到响应为xml，请求ID为“12345”嵌入响应：
```javascript
http://www.webpagetest.org/runtest.php?url=www.aol.com&runs=2&f=xml&r=12345
```

```xml
<response>
	<statusCode>200</statusCode>
	<statusText>Ok</statusText>
	<requestId>12345</requestId>
	<data>
		<testId>091111_2XFH</testId>
		<xmlUrl>http://www.webpagetest.org/xmlResult/091111_2XFH/</xmlUrl>
		<userUrl>http://www.webpagetest.org/result/091111_2XFH/</userUrl>
	</data>
</response>
```

## 二、检查测试状态
可以通过使用测试ID对`http://www.webpagetest.org/testStatus.php`执行GET请求，来检查测试的状态。
```javascript
http://www.webpagetest.org/testStatus.php?f=xml&test=your_test_id
```

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<response> 
        <statusCode>100</statusCode> 
        <statusText>Test Started</statusText> 
        <data> 
                <statusCode>100</statusCode> 
                <statusText>Test Started</statusText> 
                <testId>your_test_id</testId> 
                <runs>9</runs> 
                <fvonly>1</fvonly> 
                <location>Dulles_IE8</location> 
                <startTime>02/12/11 1:06:16</startTime> 
                <fvRunsCompleted>1</fvRunsCompleted> 
                <rvRunsCompleted>0</rvRunsCompleted> 
        </data> 
</response> 
```
+ statusCode - 200表示测试完成。 1XX表示测试仍在进行中。 和4XX表示一些错误。
+ statusText - 状态的说明
+ data - 一些测试信息包括测试ID，所请求测试的运行次数，开始时间等（非xml）

## 三、获取测试结果
在正常使用（非xml）下，将被重定向到结果页面。当使用XML API时，应该使用响应测试请求时提供的xmlUrl。 XML url也可以采用一些可选参数：

| 参数名        | 描述 |
| :------------|:-------------|
| r         | 请求ID将会在响应中显示 |
| requests  | requests = 1将请求数据包含在XML中（更慢，导致更大的响应） |
| pagespeed | pagespeed = 1在响应中包含PageSpeed分数（可能更慢） |
| domains   | domains = 1包括请求和字节的细分 |
| breakdown | breakdown = 1包括按MIME类型的请求和字节的细分 |

测试详细信息的响应与提交请求（使用不同的数据）的格式相同。所有时间均以`ms`为单位。
```xml
<response>
	<statusCode></statusCode>
	<statusText></statusText>
	<requestId></requestId>
	<data>
		<runs></runs>
		<average>
			<firstView>
			</firstView>
			<repeatView>
			</repeatView>
		</average>
		<run>
			<id></id>
			<firstView>
				<results>
				</results>
				<pages>
				</pages>
				<thumbnails>
				</thumbnails>
				<images>
				</images>
				<rawData>
				</rawData>
			</firstView>
			<repeatView>
				<results>
				</results>
				<pages>
				</pages>
				<thumbnails>
				</thumbnails>
				<images>
				</images>
				<rawData>
				</rawData>
			</repeatView>
		</run>
		<run>
		...
		</run>
	</data>
</response>
```
+ statusCode - 200表示测试完成并且结果可用。1xx表示测试仍然待处理（在合理的时间内再次尝试- 5-10秒）。 400表示无效的测试ID。
+ statusText - 失败说明
+ requestId - requestId来自于请求
+ runs - 响应中的运行数
+ average - 所有成功运行的平均测试结果（第一和重复视图数据的块）
+ run - 每个测试运行的块以及该运行的结果
+ id - 运行编号（从1开始按顺序递增）
+ firstView/repeatView - 每个用于First和Repeat视图数据的结果块
+ results - 测试结果（所有时间均以`ms`为单位）
+ pages - 到用户页面的URL
+ thumbnails - 各种图像的缩略图的URL（瀑布，清单，屏幕截图）
+ images - 全尺寸图片的URL（瀑布，清单，屏幕截图）
+ rawData - 标题和制表符分隔结果文件的URL

### 3.1 Sample
使用前面的测试请求样例（并添加一个requestId），我们将得到：
```javascript
http://www.webpagetest.org/xmlResult/091111_2XFH/?r=12345
```

```xml
<?xml version="1.0" encoding="UTF-8" ?> 
<response>
	<statusCode>200</statusCode> 
	<statusText>Ok</statusText> 
	<requestId>12345</requestId> 
	<data>
		<runs>2</runs> 
		<average>
			<firstView>
				<loadTime>4495</loadTime> 
				<TTFB>315</TTFB> 
				<bytesIn>392645</bytesIn> 
				<bytesInDoc>392645</bytesInDoc> 
				<requests>44</requests> 
				<requestsDoc>44</requestsDoc> 
				<render>1904</render> 
				<fullyLoaded>4495</fullyLoaded> 
				<docTime>4495</docTime> 
				<domTime>0</domTime> 
				<avgRun>1</avgRun> 
			</firstView>
			<repeatView>
				<loadTime>3266</loadTime> 
				<TTFB>359</TTFB> 
				<bytesIn>102151</bytesIn> 
				<bytesInDoc>102151</bytesInDoc> 
				<requests>13</requests> 
				<requestsDoc>13</requestsDoc> 
				<render>682</render> 
				<fullyLoaded>3266</fullyLoaded> 
				<docTime>3266</docTime> 
				<domTime>0</domTime> 
				<avgRun>1</avgRun> 
			</repeatView>
		</average>
		<run>
			<id>1</id> 
			<firstView>
				<results>
					<URL>http://www.aol.com</URL> 
					<loadTime>4467</loadTime> 
					<TTFB>346</TTFB> 
					<bytesOut>22403</bytesOut> 
					<bytesOutDoc>22403</bytesOutDoc> 
					<bytesIn>386528</bytesIn> 
					<bytesInDoc>386528</bytesInDoc> 
					<requests>43</requests> 
					<requestsDoc>43</requestsDoc> 
					<result>0</result> 
					<render>1963</render> 
					<fullyLoaded>4467</fullyLoaded> 
					<cached>0</cached> 
					<web>1</web> 
					<docTime>4467</docTime> 
					<domTime>0</domTime> 
					<score_cache>48</score_cache> 
					<score_cdn>96</score_cdn> 
					<score_gzip>100</score_gzip> 
					<score_cookies>87</score_cookies> 
					<score_keep-alive>94</score_keep-alive> 
					<score_minify>91</score_minify> 
					<score_combine>75</score_combine> 
					<score_compress>99</score_compress> 
					<score_etags>93</score_etags> 
					<date>1257974116</date> 
				</results>
				<pages>
					<details>http://www.webpagetest.org/result/091111_2XFH/1/details/</details> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/1/performance_optimization/</checklist> 
					<report>http://www.webpagetest.org/result/091111_2XFH/1/optimization_report/</report> 
					<breakdown>http://www.webpagetest.org/result/091111_2XFH/1/breakdown/</breakdown> 
					<domains>http://www.webpagetest.org/result/091111_2XFH/1/domains/</domains> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/1/screen_shot/</screenShot> 
				</pages>
				<thumbnails>
					<waterfall>http://www.webpagetest.org/result/091111_2XFH/1_waterfall_thumb.png</waterfall> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/1_optimization_thumb.png</checklist> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/1_screen_thumb.jpg</screenShot> 
				</thumbnails>
				<images>
					<waterfall>http://www.webpagetest.org/results/09/11/11/2XFH/1_waterfall.png</waterfall> 
					<checklist>http://www.webpagetest.org/results/09/11/11/2XFH/1_optimization.png</checklist> 
					<screenShot>http://www.webpagetest.org/results/09/11/11/2XFH/1_screen.jpg</screenShot> 
				</images>
				<rawData>
					<headers>http://www.webpagetest.org/results/09/11/11/2XFH/1_report.txt</headers> 
					<pageData>http://www.webpagetest.org/results/09/11/11/2XFH/1_IEWPG.txt</pageData> 
					<requestsData>http://www.webpagetest.org/results/09/11/11/2XFH/1_IEWTR.txt</requestsData> 
				</rawData>
			</firstView>
			<repeatView>
				<results>
					<URL>http://www.aol.com</URL> 
					<loadTime>3418</loadTime> 
					<TTFB>357</TTFB> 
					<bytesOut>8762</bytesOut> 
					<bytesOutDoc>8762</bytesOutDoc> 
					<bytesIn>108138</bytesIn> 
					<bytesInDoc>108138</bytesInDoc> 
					<requests>14</requests> 
					<requestsDoc>14</requestsDoc> 
					<result>0</result> 
					<render>682</render> 
					<fullyLoaded>3418</fullyLoaded> 
					<cached>1</cached> 
					<web>1</web> 
					<docTime>3418</docTime> 
					<domTime>0</domTime> 
					<score_cache>35</score_cache> 
					<score_cdn>83</score_cdn> 
					<score_gzip>100</score_gzip> 
					<score_cookies>66</score_cookies> 
					<score_keep-alive>83</score_keep-alive> 
					<score_minify>100</score_minify> 
					<score_combine>100</score_combine> 
					<score_compress>100</score_compress> 
					<score_etags>93</score_etags> 
					<date>1257974129</date> 
				</results>
				<pages>
					<details>http://www.webpagetest.org/result/091111_2XFH/1/details/cached/</details> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/1/performance_optimization/cached/</checklist> 
					<report>http://www.webpagetest.org/result/091111_2XFH/1/optimization_report/cached/</report> 
					<breakdown>http://www.webpagetest.org/result/091111_2XFH/1/breakdown/</breakdown> 
					<domains>http://www.webpagetest.org/result/091111_2XFH/1/domains/</domains> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/1/screen_shot/cached/</screenShot> 
				</pages>
				<thumbnails>
					<waterfall>http://www.webpagetest.org/result/091111_2XFH/1_Cached_waterfall_thumb.png</waterfall> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/1_Cached_optimization_thumb.png</checklist> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/1_Cached_screen_thumb.jpg</screenShot> 
				</thumbnails>
				<images>
					<waterfall>http://www.webpagetest.org/results/09/11/11/2XFH/1_Cached_waterfall.png</waterfall> 
					<checklist>http://www.webpagetest.org/results/09/11/11/2XFH/1_Cached_optimization.png</checklist> 
					<screenShot>http://www.webpagetest.org/results/09/11/11/2XFH/1_Cached_screen.jpg</screenShot> 
				</images>
				<rawData>
					<headers>http://www.webpagetest.org/results/09/11/11/2XFH/1_Cached_report.txt</headers> 
					<pageData>http://www.webpagetest.org/results/09/11/11/2XFH/1_Cached_IEWPG.txt</pageData> 
					<requestsData>http://www.webpagetest.org/results/09/11/11/2XFH/1_Cached_IEWTR.txt</requestsData> 
				</rawData>
			</repeatView>
		</run>
		<run>
			<id>2</id> 
			<firstView>
				<results>
					<URL>http://www.aol.com</URL> 
					<loadTime>4523</loadTime> 
					<TTFB>283</TTFB> 
					<bytesOut>22772</bytesOut> 
					<bytesOutDoc>22772</bytesOutDoc> 
					<bytesIn>398762</bytesIn> 
					<bytesInDoc>398762</bytesInDoc> 
					<requests>44</requests> 
					<requestsDoc>44</requestsDoc> 
					<result>0</result> 
					<render>1845</render> 
					<fullyLoaded>4523</fullyLoaded> 
					<cached>0</cached> 
					<web>1</web> 
					<docTime>4523</docTime> 
					<domTime>0</domTime> 
					<score_cache>48</score_cache> 
					<score_cdn>96</score_cdn> 
					<score_gzip>100</score_gzip> 
					<score_cookies>88</score_cookies> 
					<score_keep-alive>97</score_keep-alive> 
					<score_minify>91</score_minify> 
					<score_combine>75</score_combine> 
					<score_compress>98</score_compress> 
					<score_etags>93</score_etags> 
					<date>1257974140</date> 
				</results>
				<pages>
					<details>http://www.webpagetest.org/result/091111_2XFH/2/details/</details> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/2/performance_optimization/</checklist> 
					<report>http://www.webpagetest.org/result/091111_2XFH/2/optimization_report/</report> 
					<breakdown>http://www.webpagetest.org/result/091111_2XFH/2/breakdown/</breakdown> 
					<domains>http://www.webpagetest.org/result/091111_2XFH/2/domains/</domains> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/2/screen_shot/</screenShot> 
				</pages>
				<thumbnails>
					<waterfall>http://www.webpagetest.org/result/091111_2XFH/2_waterfall_thumb.png</waterfall> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/2_optimization_thumb.png</checklist> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/2_screen_thumb.jpg</screenShot> 
				</thumbnails>
				<images>
					<waterfall>http://www.webpagetest.org/results/09/11/11/2XFH/2_waterfall.png</waterfall> 
					<checklist>http://www.webpagetest.org/results/09/11/11/2XFH/2_optimization.png</checklist> 
					<screenShot>http://www.webpagetest.org/results/09/11/11/2XFH/2_screen.jpg</screenShot> 
				</images>
				<rawData>
					<headers>http://www.webpagetest.org/results/09/11/11/2XFH/2_report.txt</headers> 
					<pageData>http://www.webpagetest.org/results/09/11/11/2XFH/2_IEWPG.txt</pageData> 
					<requestsData>http://www.webpagetest.org/results/09/11/11/2XFH/2_IEWTR.txt</requestsData> 
				</rawData>
			</firstView>
			<repeatView>
				<results>
					<URL>http://www.aol.com</URL> 
					<loadTime>3113</loadTime> 
					<TTFB>360</TTFB> 
					<bytesOut>7426</bytesOut> 
					<bytesOutDoc>7426</bytesOutDoc> 
					<bytesIn>96163</bytesIn> 
					<bytesInDoc>96163</bytesInDoc> 
					<requests>11</requests> 
					<requestsDoc>11</requestsDoc> 
					<result>0</result> 
					<render>682</render> 
					<fullyLoaded>3113</fullyLoaded> 
					<cached>1</cached> 
					<web>1</web> 
					<docTime>3113</docTime> 
					<domTime>0</domTime> 
					<score_cache>25</score_cache> 
					<score_cdn>66</score_cdn> 
					<score_gzip>100</score_gzip> 
					<score_cookies>58</score_cookies> 
					<score_keep-alive>77</score_keep-alive> 
					<score_minify>100</score_minify> 
					<score_combine>100</score_combine> 
					<score_compress>100</score_compress> 
					<score_etags>91</score_etags> 
					<date>1257974152</date> 
				</results>
				<pages>
					<details>http://www.webpagetest.org/result/091111_2XFH/2/details/cached/</details> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/2/performance_optimization/cached/</checklist> 
					<report>http://www.webpagetest.org/result/091111_2XFH/2/optimization_report/cached/</report> 
					<breakdown>http://www.webpagetest.org/result/091111_2XFH/2/breakdown/</breakdown> 
					<domains>http://www.webpagetest.org/result/091111_2XFH/2/domains/</domains> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/2/screen_shot/cached/</screenShot> 
				</pages>
				<thumbnails>
					<waterfall>http://www.webpagetest.org/result/091111_2XFH/2_Cached_waterfall_thumb.png</waterfall> 
					<checklist>http://www.webpagetest.org/result/091111_2XFH/2_Cached_optimization_thumb.png</checklist> 
					<screenShot>http://www.webpagetest.org/result/091111_2XFH/2_Cached_screen_thumb.jpg</screenShot> 
				</thumbnails>
				<images>
					<waterfall>http://www.webpagetest.org/results/09/11/11/2XFH/2_Cached_waterfall.png</waterfall> 
					<checklist>http://www.webpagetest.org/results/09/11/11/2XFH/2_Cached_optimization.png</checklist> 
					<screenShot>http://www.webpagetest.org/results/09/11/11/2XFH/2_Cached_screen.jpg</screenShot> 
				</images>
				<rawData>
					<headers>http://www.webpagetest.org/results/09/11/11/2XFH/2_Cached_report.txt</headers> 
					<pageData>http://www.webpagetest.org/results/09/11/11/2XFH/2_Cached_IEWPG.txt</pageData> 
					<requestsData>http://www.webpagetest.org/results/09/11/11/2XFH/2_Cached_IEWTR.txt</requestsData> 
				</rawData>
			</repeatView>
		</run>
	</data>
</response>
```

## 四、取消测试
使用测试ID（如果需要API密钥），如果它没有开始运行，你可以取消测试，。
```javascript
http://www.webpagetest.org/cancelTest.php?test=<testId>&k=<API key>
```

## 五、地点信息
可以使用getLocations.php接口请求位置列表以及每个位置的待处理测试数量：
```javascript
http://www.webpagetest.org/getLocations.php?f=xml
```

```xml
<response>
	<statusCode>200</statusCode>
	<statusText>Ok</statusText>
	<data>
		<location>
			<id>Dulles_IE7</id>
			<Label>Dulles, VA USA</Label>
			<Browser>IE 7</Browser>
			<default>1</default>
			<PendingTests>
				<Total>0</Total>
				<HighPriority>0</HighPriority>
				<LowPriority>0</LowPriority>
			</PendingTests>
		</location>
		<location>
			<id>Dulles_IE8</id>
			<Label>Dulles, VA USA</Label>
			<Browser>IE 8</Browser>
			<PendingTests>
				<Total>0</Total>
				<HighPriority>0</HighPriority>
				<LowPriority>0</LowPriority>
			</PendingTests>
		</location>
	</data>
</response>
```