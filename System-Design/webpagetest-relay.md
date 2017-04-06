# WebPagetest中继

目前的系统架构有每个测试代理直接连接到WebPagetest服务器，用于检索工作和发布结果：  

![](/assets/img/system/replay1.png)

这适用于很多用例，但它要求所有代理可访问服务器，并且每个代理只能为单个服务器运行测试。添加对中继服务器的支持是有用的，该中继服务器一方面公开相同的代理接口，另一方面为WebPagetest服务器提供API来进行通信。  
在简单的用例中，这将允许继电器放置在不安全的位置（例如公共互联网）。但服务器本身可以在防火墙或其他安全区域内运行，在公共互联网上以及防火墙内都有测试代理：

![](/assets/img/system/replay2.png)

假设WebPagetest服务器上每个定义的“位置”都可以引用不同的中继服务器，并且每个中继服务器可以支持连接到它的多个服务器（使用基于密钥的身份验证），这样可以打开很多有趣的可能性：
+ 多个系统可以共享测试代理池（拥有控制对它们的访问的中继器）。 分享可以是直接的协作或更复杂的地方，每个用户对于他们的使用份额进行收费，甚至是直接订阅模式，其中所有者以每个测试的方式销售能力
+ 中继服务器可以优先处理服务器请求测试的请求（基于任何对系统有意义的优先级排序）
+ 给定的WebPagetest实例可以利用公共和私有测试代理，甚至通过使用多个继电器从多个公共代理池中抽取

![](/assets/img/system/replay3.png)

### 实施细节

任何完整的WebPagetest实例可以作为中继器运行，并进行一些调整
+ `runtest.php` - 添加对直接上传的测试作业的支持（并将作业绑定到所使用的API密钥）
+ `common.inc` - `getTestPath()` - 组合API密钥和测试ID，形成新的测试ID，并将测试结果存储在不同的树中
+ `downloadtest.php` - 添加一个新界面来下载给定测试的zip存档（基本上与发布相同，但在`pull`模式中）
+ `deletetest.php` - 添加一个新的界面来显式删除测试（基于匹配的API键）

WebPagetest Interface
+ Any location in locations.ini can be configured to talk to a relay server with the following settings:
    + Relay server URL (base URL)
    + Relay location ID (relay server's location ID to use)
    + Relay Key (API Key to use when interfacing with the relay server)
+ When tests are submitted to the local server, instead of writing the test job to a local file it will post it to the relay server
+ When checking the status of a test, if the test is not complete then WebPagetest will ask the relay server for the status
+ If the relay server indicates that the test is complete, WebPagetest will download the result from the relay server as a zip and extract it locally
    + After download/extract WPT will indicate that it is done with the test so the relay can delete it's copy

Agent Interface
+ Agents will communicate with a relay exactly as if they are communicating with WebPagetest (work/getwork.php, work/resultimage.php, work/workdone.php)

Relay Implementation
+ A dedicated relay server will just be a subset of a full WPT install without the UI support (SVN will be used to place the selected files into a wptrelay directory in the dist release)