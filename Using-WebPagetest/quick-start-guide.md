# 快速入门指南
WebPagetest的核心是用于测量和分析网页的性能。有很多选项，看着很吓人，但其实做快速测试是很简单的。
本指南将引导您提交测试和结果解释。

##一、 Running a Performance Test
###1.1 Enter The Page URL:
###1.2 Select a Location:
###1.3 Select a Browser:
###1.4 Submit the Test
##二、 Interpreting the Results
###2.1 Optimization Grades:
####2.1.1 Keep-alive Enabled:
####2.1.2 Compress Text:
####2.1.3 Compress Images:
####2.1.4 Cache Static Content:
####2.1.5 Combine JS/CSS Files:
####2.1.6 Use of CDN:
###2.2 High-level Metrics:
####2.2.1 Repeat View:
####2.2.2 Document Complete:
####2.2.3 Fully Loaded:
####2.2.4 Load Time:
####2.2.5 First Byte:
####2.2.6 Start Render:
####2.2.7 DOM Elements:
####2.2.8 The DOM Elements metric is the count of the DOM elements on the tested page as measured at the end of the test.
####2.2.9 Requests:
####2.2.10 Bytes In:

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

![](/assets/img/using/guide/url.png)