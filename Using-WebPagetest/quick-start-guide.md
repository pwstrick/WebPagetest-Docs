# 快速入门指南
WebPagetest的核心是用于测量和分析网页的性能。有很多选项，看着很吓人，但其实做快速测试是很简单的。
本指南将引导您完成提交测试和解释结果。

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