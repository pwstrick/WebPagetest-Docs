# 快速入门指南
WebPagetest的核心是用于测量和分析网页的性能。有很多选项，看着很吓人，但其实做快速测试是很简单的。
本指南将引导你提交测试和结果解释。

##一、运行性能测试(Running a Performance Test)
###1.1 输入网页网址(Enter The Page URL)

你需要做的第一件事是决定一个页面来测试。大多数人从他们的网站的主页开始（但不要忽视人们访问的其他页面）。确定要测试的页面后，转到WebPagetest并为其指定要测试的页面的URL：

![](/assets/img/using/guide/url.png)
###1.2 选择位置(Select a Location)
接下来，应该决定从哪里运行测试。WebPagetest具有位于世界各地的测试机器，你应该从接近用户访问的位置进行测试，从列表中选择一个位置，或者单击`Select from Map`按钮，从地图视图中选择一个位置（只需单击气球，然后确定）。如果将指针放在气泡上，它们将显示一条消息，提示位置信息：

![](/assets/img/using/guide/map.png)
###1.3 选择浏览器(Select a Browser)
最后，需要决定使用什么浏览器进行测试。**不同的位置支持不同的浏览器**，如果给定的位置没有正在寻找的浏览器，可以尝试不同的位置。 Dulles，VA USA位置支持WebPagetest工作的所有浏览器（IE 6,7,8和9）。现在忽略“dynaTrace”浏览器，这些用于更高级的分析。我们通常建议使用IE7进行初始测试，因为它几乎是最糟糕的情况，并且更容易看到很多问题，所以如果你不知道什么浏览器，开始只需使用IE7。

![](/assets/img/using/guide/browser.png)
###1.4 提交测试(Submit the Test)
一切配置完成后，点击`START TEST`按钮，请求将发送到测试位置进行测试。测试可能需要一段时间才能运行，具体取决于有多少次测试（在测试之前至少有一分钟的测试时间，但是它的时间甚至更长）。一旦测试完成，你将得到结果。

##二、解释结果(Interpreting the Results)
第一次看到结果信息可能有点吓人，信息量有点大，但首先可以先查看一些关键信息。

###2.1 优化等级(Optimization Grades)
在结果页面的顶部是一组最关键的性能优化等级。涵盖了适用于所有网站的基本优化，任何不是A或B的都需要进行进一步的优化。

![](/assets/img/using/guide/grade.png)
####2.1.1 长连接已启动(Keep-alive Enabled)
####2.1.2 压缩文本(Compress Text)
####2.1.3 压缩图片(Compress Images)
####2.1.4 缓存静态内容(Cache Static Content)
####2.1.5 合并JS/CSS文件(Combine JS/CSS Files)
####2.1.6 使用CDN(Use of CDN)
###2.2 高级度量(High-level Metrics)
####2.2.1 重复视图(Repeat View)
####2.2.2 document.onload事件触发时间(Document Complete)
####2.2.3 页面所有元素加载花费时间(Fully Loaded)
####2.2.4 整页加载时间(Load Time)
####2.2.5 第一个字节加载所需时间(First Byte)
####2.2.6 页面渲染时间(Start Render)
####2.2.7 DOM元素数量(DOM Elements)
####2.2.8 DOM元素度量标准是在测试结束时测量的测试页面上的DOM元素的计数(The DOM Elements metric is the count of the DOM elements on the tested page as measured at the end of the test)
####2.2.9 HTTP请求数(Requests)
####2.2.10 字节输入(Bytes In)

```javascript
common.const = {
  COOKIE_APP_NAME: 'chelun_appName', //车轮APP名称，比如 查违章，车轮社区
  COOKIE_APP_VERSION: 'chelun_appVersion', //APP版本号
  COOKIE_APP_TOKEN: 'chelun_acToken', //车轮统一登录态，去服务端校验
  COOKIE_DEVICE: 'chelun_device', //机型，主要针对安卓，读取系统设备信息来做兼容性判断和数据统计
  COOKIE_OS_TYPE: 'chelun_osType', //操作系统 ios android
  COOKIE_OS_VERSION: 'chelun_osVersion', //IOS版本号   安卓版本号
  COOKIE_IS_LOGIN: 'chelun_isLogin', //是否登录
};
```

```php
<?php
class activityController extends \library\controller\Web {
}
```

`文字高亮`

1. 开闭原则
* 里氏转换原则
* 依赖倒转原则
* 接口隔离原则
* 组合/聚合复用原则
* “迪米特”法则
* 单一职责原则

横线
---

| 左对齐 | 居中  | 右对齐 |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |

