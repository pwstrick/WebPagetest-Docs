# 快速开始
> At its core, WebPagetest is used for measuring and analyzing the performance of web pages.  There are a lot of options that may seem intimidating at first but doing quick testing is pretty simple.  This guide will walk you through submitting a test and interpreting the results.

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

| 左对齐 | 居中  | 右对齐 |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |

![](/assets/img/using/guide/url.png)