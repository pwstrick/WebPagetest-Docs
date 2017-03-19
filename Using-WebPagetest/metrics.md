# 指标

### 1. 网页级指标(Page-level Metrics)
这些是为整个页面捕获并显示的顶级度量值。

### 2. 整页加载时间(Load Time)
测量的时间是从初始化请求，到开始执行`window.onload`事件。
>The Load Time is measured as the time from the start of the initial navigation until the beginning of the window load event (onload).

### 3. 页面所有元素加载时间(Fully Loaded)
从初始化请求，到Document Complete后，2秒内（中间几百毫秒轮询）没有网络活动的时间，但这2秒是不包括在测量中的，所以会出现两个差值大于或小于2秒。
> The Fully Loaded time is measured as the time from the start of the initial navigation until there was 2 seconds of no network activity after Document Complete.  This will usually include any activity that is triggered by javascript after the main page loads.

### 4. 第一个字节加载时间(First Byte)
第一个字节时间（通常缩写为TTFB）被测量为从初始化请求，到服务器响应的第一个字节，被浏览器接收的时间（不包括DNS查询、TCP连接的时间）。  
我理解TTFB的计算是从下图中RequestStart到ResponseStart这之间的时间。

![](/assets/img/using/guide/performance.png)


### 5. 页面渲染时间(Start Render)
测量的时间是从初始化请求，到第一个内容被绘制到浏览器显示的时间。在瀑布图中有两个参数指标`Start Render`和`msFirstPaint`。
+ `Start Render`是通过捕获页面加载的视频，并在浏览器第一次显示除空白页之外的其他内容时查看每个帧来衡量的。它只能在实验室测量，通常是最准确的测量。
+ `msFirstPaint`（IE专用属性）是由浏览器本身报告的一个测量，它认为绘制的第一个内容。通常是相当准确，但有时它报告的时候，浏览器只画一个空白屏幕。

### 6. 首屏展现平均值(Speed Index)
表示页面呈现用户可见内容的速度（越低越好）。有关如何计算的更多信息，请参见：[Speed Index](/Using-WebPagetest/metrics-speed-index.md)。

### 7. DOM元素数量(DOM Elements)
在测试结束时测试页面上的DOM元素的计数。

### 8. 请求级度量标准(Request-level Metrics)
这些是为每个请求捕获和显示的度量。