# 网页重播

## 一、背景
[网页重放](https://github.com/chromium/web-page-replay)可以记录实时网页，并在本地重播以进行测试。对于WebPageTest，Replay用于通过记录网站并在更可靠的条件下播放来规范化访问活动网站的结果。重播站点可减少由网络挂起，网络服务器流量峰值以及可能影响基准测试结果的其他因素造成的异常值。设置WebPageReplay非常容易，强烈建议你充分利用WPT私人实例。

## 二、要求
+ Python 2.6
+ Port 80
+ Port 53 UDP和TCP支持DNS流量
网络调节：有很多依赖关系在WPR级别启用网络调节，请参阅[WPR入门](https://github.com/chromium/web-page-replay/blob/master/documentation/GettingStarted.md)。WPT的浏览器代理做网络调节，所以这些步骤可以跳过。

## 三、服务器步骤
1. 检查服务器上的代码
2. 创建供服务器使用的wpr存档
3. 在服务器模式下启动WPR（连接到端口80/53所需的root权限）。 我建议用nohup，screen或supervisord启动它。

```bash
$ git clone https://github.com/chromium/web-page-replay.git
$ sudo ./replay.py --record archive.wpr
$ sudo screen 
./replay.py -M ~/archive.wpr
```
在这一点上，你应该有一个工作的服务器。现在可以将DNS设置为此框的IP，应该在服务器上的控制台中看到流量。访问的每个页面都显示错误404，因为我们没有记录。

## 四、浏览器步骤
现在我们有一个工作的WPR服务器，让我们设置一个浏览器代理使用它。幸运的是，WPT已经与WPR具有良好的集成性。测试代理将服务器设置为记录模式，加载网页，然后将WPR置于重放模式并对其运行测试。这都可以从WPR服务器上的控制台查看。

#### 4.1 在`wptdriver.ini`中指定网页显示`IP ADDRESS`
我没有在IE8 / IE9 URL Blast测试。
    web_page_replay_host=XXX.XXX.XXX.XXX

你的配置应该看起来像这样。
```bash
[WebPagetest]
url=http://wpt-rmn.cloudapp.net/
location=agent-location
browser=chrome
Time Limit=120
;key=TestKey123
;Automatically install and update support software (Flash, Silverlight, etc)
software=http://www.webpagetest.org/installers/software.dat
web_page_replay_host=XXX.XXX.XXX.XXX

[chrome]
exe="%PROGRAM_FILES%\Google\Chrome\Application\chrome.exe"
options='--load-extension="%WPTDIR%\extension" --user-data-dir="%PROFILE%" --no-proxy-server'
installer=http://www.webpagetest.org/installers/browsers/chrome.dat

[Firefox]
exe="%PROGRAM_FILES%\Mozilla Firefox\firefox.exe"
options='-profile "%PROFILE%" -no-remote'
installer=http://www.webpagetest.org/installers/browsers/firefox.dat
template=firefox

[Safari]
exe="%PROGRAM_FILES%\Safari\Safari.exe"

[IE 11]
exe="C:\Program Files\Internet Explorer\iexplore.exe"
```

#### 4.2 这应该是所有必要的。现在，当您对此代理运行测试时，它将通过webpagereplay服务器发送流量。立即运行测试，并确保这两行显示在WPR服务器上的控制台/日志中。
    2014-07-07 18:11:12,885 DEBUG Served: GET http://XXX.XXX.XXX.XXX/web-page-replay-command-record [('cache-control', 'no-cache'), ('host', 'XXX.XXX.XXX.XXX')] (0ms)
    2014-07-07 18:11:32,055 DEBUG Served: GET http://XXX.XXX.XXX.XXX/web-page-replay-command-replay [('cache-control', 'no-cache'), ('host', 'XXX.XXX.XXX.XXX')] (0ms)

## 五、FAQ