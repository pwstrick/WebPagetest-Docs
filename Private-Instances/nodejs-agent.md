# Node.js代理

## 一、概述
Node.js代理支持桌面浏览器和移动设备。
支持的浏览器：
+ 桌面Chrome（通过chromedriver2）
+ Android Chrome（通过chromedriver2）
+ iOS移动Safari（通过[ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy)）

其他功能包括：
+ Android on-device tcpdump支持（通过tcpdump apk）。
+ Chrome WebDriver脚本支持（iOS是待处理的[ios驱动程序](https://github.com/ios-driver/ios-driver)）。
+ 通过用户定义的“捕获”脚本进行视频捕获。

## 二、文件
+ [如何安装代理](/Private-Instances/agent-setup.md)
+ [如何编写异步JavaScript代码](/Developer-Information/async-js.md)