# 指标

### 1. 网页级指标(Page-level Metrics)
这些是为整个页面捕获并显示的顶级度量值。

### 2. 整页加载时间(Load Time)
测量的时间是从开始浏览页面，到DOM加载事件（document.onload）执行的这段时间。

### 3. 页面所有元素加载时间(Fully Loaded)
测量的时间是从开始浏览页面，到Document Complete后，2秒内没有网络活动的时间，`我的理解是window.onload`。

### 4. 第一个字节加载时间(First Byte)
第一个字节时间（通常缩写为TTFB）被测量为从开始浏览页面，到服务器响应的第一个字节，被浏览器接收的时间。

### 5. 页面渲染时间(Start Render)
测量的时间是从开始浏览页面，到第一个内容被绘制到浏览器显示的时间。

### 6. 首屏展现平均值(Speed Index)
表示页面呈现用户可见内容的速度（越低越好）。有关如何计算的更多信息，请参见：[Speed Index](/Using-WebPagetest/metrics-speed-index.md)。

### 7. DOM元素数量(DOM Elements)
在测试结束时测试页面上的DOM元素的计数。

### 8. 请求级度量标准(Request-level Metrics)
这些是为每个请求捕获和显示的度量。