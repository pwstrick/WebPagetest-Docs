# 概述

以下是WebPagetest的整体系统设计：

![](/assets/img/system/overview.png)

所有通信均通过HTTP完成，箭头方向表示HTTP请求的方向。  
测试代理人轮询找到工作的服务器，并在测试完成后将结果发回服务器。

有关特定系统的更多信息，请参阅相关文档（即将推出）：  
+ 服务器架构
    + 自动化API
    + 代理API
+ 代理架构
    + 旧代理（IE8以下使用的UrlBlast）
    + 新代理（Chrome/Firefox等使用的WPTDriver）