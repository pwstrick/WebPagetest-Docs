# 私有实例
WebPagetest可安装和运行私有实例的软件包。

## 一、发布
最新版本是[3.0](https://github.com/WPO-Foundation/webpagetest/releases/tag/WebPageTest-3.0)

## 二、轻松部署（在EC2上）
有一个[EC2 AMI](https://github.com/WPO-Foundation/webpagetest/blob/master/docs/EC2/Server%20AMI.md)用于WebPageTest服务器，可以有很多好处：
+ 无需配置（在5分钟内启动并运行）。
+ 根据需要在所有EC2（亚马逊弹性计算云）区域中自动启动和停止EC2测试代理。
+ 自动更新到最新的服务器和代理代码。

## 三、手动部署
### 3.1 系统要求
WebPageTest可以配置在一个系统上运行（Web服务器和测试机器都在同一台PC上运行），或者为Web服务器和测试人员使用单独的系统。

### 3.2 Web服务器
Web服务器可以是任何支持PHP的操作系统（Linux或Windows）。
+ Nginx (推荐php-fpm) 或者 Apache 2.x+:
+ PHP 5.3.0 或更新版本，包括以下模块:
    + gd
    + zip
    + zlib
    + mbstring
    + curl (如果你想使用远程存储，像S3和谷歌存储)
    + sqlite (如果你想要能够编辑测试标签)
+ ffmpeg 
    + 在路径中可用（用于提取视频缩略图）
    + 对于移动代理视频支持，需要1.x（Linux，Windows）
+ imagemagick (可选)
    + 如果使用移动Node.js代理，则需要移动视频处理
+ jpegtran (可选)
    + 在路径中可用（用于jpeg分析）
+ exiftool (可选)
    + 在路径中可用（用于jpeg分析）

### 3.3 测试机
Windows（Vista或更高版本）（如果使用64位，需要WebPagetest 2.9或更高版本支持流量整形）

## 四、Configuring
`zip`存档包含2个文件夹。`www`文件夹是Web服务器软件，代理文件夹用于测试机器。存档中的配置文件具有`.sample`扩展名，因此如果要更新现有安装，则可以使用存档中的新文件覆盖当前文件。  
`2.8`的新版本是一个脚本，将检查常见的配置问题。 可以通过`http://<your web server>/install`访问。

### 4.1 Web Server
1. 使用所需的模块配置Apache，并设置为允许覆盖.htaccess。配置文件可能如下所示：
```bash
<Directory "/var/www/webpagetest">
        AllowOverride all
        Order allow,deny
        Allow from all
</Directory>
<VirtualHost *:80>
        DocumentRoot /var/www/webpagetest
</VirtualHost>
```
2. 如果将使用代理，需要上传`.pcap`文件。在`php.ini`中将`upload_max_filesize`和`post_max_size`设置为大值（`10mb`应该足够）。  
3. 如果要收集Chrome开发工具跟踪，请考虑将`memory_limit`设置为较大值或通过将其设置为-1来禁用内存限制。    
4. 使用`PHP DSO`处理程序`mod_php`可以大幅减少CPU使用率。  
5. 重新启动Apache以使用新的配置设置。  
6. 将文件从归档中的`www`文件夹复制到DocumentRoot位置（例如`/var/www/webpagetest`）。  
7. 授予Apache用户对DocumentRoot下的以下文件夹的读/写权限：
    + tmp
    + dat
    + results
    + work/jobs
    + work/video
    + logs
8. 在设置文件夹中有几个设置文件可用于配置站点。制作所有`.sample`文件的副本（删除`.sample`扩展名）作为配置设置的模板。大多数设置可以按原样使用，除了`locations.ini`（特别是如果要配置多个测试位置）。  
9. 有关配置`locations.ini`的详情，请访问：[locations](/Private-Instances/locations.md)

### 4.2 测试机
1. 安装要用于测试的浏览器  
2. 在测试系统中配置的管理员帐户。最简单和最可靠的方法是使用`Microsoft Technet`的[自动登录应用程序](https://sites.google.com/a/webpagetest.org/docs/private-instances)。  
3. 禁用任何屏幕保护程序（桌面需要保持可见的视频捕获工作）  
4. 停用UAC（Vista或更高版本 - 滑动到“从不通知”）  
5. 卸载IE的增强安全模式（Windows Server）  
6. 在虚拟机（特别是KVM）中运行时使用稳定的时钟
    + `/USEPMTIMER到XP`或`Server 2003`的`boot.ini`
    + 管理员shell命令“bcdedit / set {default} useplatformclock true”
7. 配置Windows引导到桌面（Server 2012）
    + 安装桌面体验
    + [http://www.win2012workstation.com/tag/start-screen/#manual](http://www.win2012workstation.com/tag/start-screen/#manual)
8. 禁用事件跟踪器（Windows Server - 为了方便）
    + 运行`gpedit.msc`
    + 打开`Computer Configuration\Administrative Templates\System`
    + 打开`Display Shutdown Event Tracker`
    + 将其设置为禁用
9. 将测试软件从代理文件夹复制到系统（本示例为`c:\ webpagetest`）  
10. 对于`Windows 8.1`（或服务器2012 R2）测试代理程序，请安装`DUMMYNET ipfw驱动程序`
+ 如果要在64位Windows上安装，请右键单击`c:\webpagetest\dummynet\64bit`文件夹中的`testmode.cmd`，然后选择`以管理员身份运行`。重新启动系统以启用测试模式。如果你不运行，重启后不会开启流量整形。
+ 将文件从`webpagetest\dummynet\32bit`或`webpagetest\dummynet\64bit`目录复制到`webpagetest\dummynet`目录（取决于操作系统）
+ 拔出用于访问Internet的网络适配器的属性 
+ 点击`Install`
+ 选择`Service`然后点击`Add`
+ 点击`Have Disk`并导航到`webpagetest\dummynet`
+ 选择`ipfw + dummynet服务`（单击任何有关未签名的驱动程序的警告）
11. Windows 10中，如果使用Microsoft Edge测试（工作进行中）：
+ 为所有用户安装[Python 2.7](https://www.python.org/downloads/)到`c:\ Python27`（默认安装目录）
+ 从cmd shell安装selenium
    + c:\Python27\Scripts\pip install selenium
+ 从代理文件夹安装`pyWin32`（可能需要从管理员命令提示符运行）
+ 安装[Imagemagick](https://www.imagemagick.org/script/binary-releases.php#windows)
+ 安装[Windows性能工具包](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/test/wpt/index?f=255&MSPPError=-2147217396)（取消选中adk设置中的所有其他选项）
12. 需要为每个版本的IE配置单独的机器/VM，但其余的浏览器都可以在同一台机器上运行，与IE共享一台机器（测试将不会同时运行，将会交替进行）  
13. 创建任务计划程序任务以在登录时运行`wptdriver.exe`：
+ 将其配置为以最高权限运行
+ 让它在用户帐户登录时运行
+ 确保它未配置为在3天后终止（默认任务调度程序配置）
14. 根据样本创建设置文件（`wptdriver.ini`）的副本
+ `wptdriver`可以自动安装Chrome和Firefox（并保持Firefox最新）。如果你想手动安装浏览器，然后删除installer = ...条目为每个你想要tp手动安装的浏览器
+ 确保浏览器可执行文件的路径对于系统是正确的（如果正在自动安装Chrome和Firefox，他们只会在首次运行`wptdriver`后安装）
+ 将位置配置为与`locations.ini`中服务器上定义的位置匹配
+ 配置位置键与`locations.ini`中的服务器匹配
+ 确保ini文件中的可用浏览器匹配位置的browsers = x，y，z条目中的locations.ini中定义的列表。
15. 启动`wptdriver`，安装它需要安装的任何软件（退出，当出现“waiting for work”）。  
16. 重新启动以确保一切正常启动    

如果将远程桌面连接到测试机，请确保在完成后重新启动机器，否则桌面将保持锁定，屏幕捕获将不起作用。

#### 4.2.1 无外设服务器（包括Google Compute Engine）
屏幕截图和视频捕获都需要桌面可见才能进行测试。 某些服务器没有可用的视频设备或分辨率太低，无法用于测试。在这些情况下，设置变得有点复杂（但仍然可能）。测试机需要2个用户帐户，并且需要为自己运行RDP会话，并在RDP会话中运行测试，这将给它一个虚拟显示使用。这也可以用作一种安全技术，如果你不舒服让桌面解锁在任何时候。
+ 1. 创建2个用户帐户。 我们将他们称为“用户”和“管理员”。 “用户”帐户将是在启动时用于创建RDP会话的帐户，“管理员”帐户是将运行测试的地方。 “用户”帐户不需要是管理员帐户
+ 2. 设置测试帐户：
    + 将RDP连接到“管理员”帐户，并将其设置为使用正常的WPT配置运行测试
    + 注销RDP会话（不要仅断开连接，将用户注销，但使系统保持运行）
    + 通过RDP重新连接到“管理员”帐户，并确保测试在连接后立即自动启动并正常运行
+ 3. 设置RDP主机帐户：
    + 用RDP连接到“用户”帐户
    + 配置自动登录在启动时自动登录“用户”帐户。我通常使用Microsoft的[自动登录应用程序](https://technet.microsoft.com/en-us/sysinternals/autologon.aspx)来进行配置。
    + 使用管理员帐户将RDP连接到127.0.0.2，并记住凭据（不能是localhost或127.0.0.1，因为RDP将阻止这些尝试，但它允许127.0.0.2仍然是本地主机）
    + 打开cmd shell
    + 使用`cmdkey /generic:TERMSRV/127.0.0.2 /user:administrator /pass:<adminpass>`存储RDP的管理员密码
    + 输入`mstsc /w:1920 /h:1200 /v:127.0.0.2`并确保它打开一个RDP连接并开始测试（在验证其工作后关闭会话）
    + 创建任务计划程序任务以在运行mstsc.exe的用户帐户的命令行选项`/w:1920 /h:1200 /v:127.0.0.2`登录时运行。

现在当你重新启动服务器，它应该自动登录到“用户”帐户，然后RDP到本地实例与“管理员”帐户，测试将在RDP会话内部运行。
### 4.3 Mobile
移动代理说明在[这里可用](/Private-Instances/agent-setup.md)。

## 五、EC2测试代理
我们为EC2准备了公共AMI，可以用作通过实例用户数据动态配置的WebPagetest测试器。这些映像具有安装和配置测试系统所需的所有软件（包括用于生成视频的`AVISynth`和用于进行流量整形的`dummynet`）。

### 5.1 配置
在启动实例时，通过用户数据字符串来配置测试代理。

#### 5.1.1 参数
+ wpt_server - WebPagetest正在运行的Web服务器（必需）
+ wpt_url - （仅限Linux代理）用于获取的工作目录的基本URL。即`https://www.webpagetest.org/work/`
+ wpt_loc - 要用于`wptdriver`的位置名称（可选 - 如果未指定，它将回退到`wpt_location`或从区域构建 - 例如`ec2-us-east`）
+ wpt_location - The location name to use for URLBlast (optional - if not specified it will be built from the region and browser - ec2-us-east-IE8 for example).  wptdriver will append _wptdriver to this location ID if wpt_loc is not set.
+ wpt_key - secret key for the specified location (optional)
+ wpt_timeout - timeout setting for each test in seconds (optional, defaults to 60)
+ wpt_username - username for basic authentication with WPT server. Ignored if wpt_password is not specified. (optional)
+ wpt_password - password for basic authentication with WPT server. Ignored if wpt_username is not specified. (optional)
+ wpt_validcertificate - Ignored unless the scheme of the wpt_server is 'https' If the value is '0', the hostname and expiry of the certificate are not checked. If the value is '1', the CN of the WPT server SSL certificate must match the hostname specified in the wpt_server parameter, and the certificate must be valid. (optional, defaults to '0')

#### 5.1.2 Example User Data string
wpt_server=www.webpagetest.org wpt_loc=Test wpt_key=xxxxx

![](/assets/img/using/private/ec2config.png)

#### 5.1.3 Sample locations.ini
A sample locations.ini for the server is available that is configured with all of the available EC2 regions: [http://webpagetest.googlecode.com/svn/trunk/www/webpagetest/settings/locations.ini.EC2-sample](http://webpagetest.googlecode.com/svn/trunk/www/webpagetest/settings/locations.ini.EC2-sample)

### 5.2 AMI Images
The wptdriver instances (IE 9, 10 and 11) must have a matching browser name in locations.ini for the version of IE in the AMI ("IE 9", "IE 10" or "IE 11").  
The instances will automatically install the latest supported versions of Chrome and Firefox when they start up (and will automatically update while they are running)

#### 5.2.1 Finding the Images (aka EC2 AMI search sucks)
If you are having trouble finding the AMI's for manual launching, it's because EC2's AMI search is pretty broken.  I've found this path to work:
+ Do NOT search for the AMI in the Images->AMIs interface
+ Go to Instances->Instances (or spot instances)
+ Launch Instance
+ Select "Community AMIs"
+ Check "Windows"
+ Put the AMI ID in the search box and hit enter

#### 5.2.2 us-east (Virginia)
    IE9/Chrome/Firefox/Safari - ami-83e4c5e9  
    IE10/Chrome/Firefox/Safari - ami-0ae1c060  
    IE11/Chrome/Firefox/Safari - ami-4a84a220  
    Linux (Chrome Only) - ami-cf68c6d9

#### 5.2.3 us-east-2 (Ohio)
    IE9/Chrome/Firefox/Safari - ami-c86933ad  
    IE10/Chrome/Firefox/Safari - ami-55742e30  
    IE11/Chrome/Firefox/Safari - ami-c96933ac  
    Linux (Chrome Only) - ami-cd4f6ba8
    
#### 5.2.4 us-west (California)
    IE9/Chrome/Firefox/Safari - ami-03d6a263  
    IE10/Chrome/Firefox/Safari - ami-05eb9f65  
    IE11/Chrome/Firefox/Safari - ami-678afe07  
    Linux (Chrome Only) - ami-2a3c644a

#### 5.2.5 us-west-2 (Oregon)
    IE9/Chrome/Firefox/Safari - ami-03e80c63  
    IE10/Chrome/Firefox/Safari - ami-fdeb0f9d  
    IE11/Chrome/Firefox/Safari - ami-b4ab4fd4  
    Linux (Chrome Only) - ami-9cc049fc

#### 5.2.6 ca-central-1 (Canada Central)
    IE9/Chrome/Firefox/Safari - ami-184efc7c  
    IE10/Chrome/Firefox/Safari - ami-13328077  
    IE11/Chrome/Firefox/Safari - ami-0345f767  
    Linux (Chrome Only) - ami-4140fd25
    
#### 5.2.7 eu-west-1 (Ireland)
    IE9/Chrome/Firefox/Safari - ami-2d5fea5e  
    IE10/Chrome/Firefox/Safari - ami-3b45f048  
    IE11/Chrome/Firefox/Safari - ami-a3a81dd0  
    Linux (Chrome Only) - ami-cc1423aa
    
#### 5.2.8 eu-west-2 (London)
    IE9/Chrome/Firefox/Safari - ami-4ad6dc2e  
    IE10/Chrome/Firefox/Safari - ami-2dd5df49  
    IE11/Chrome/Firefox/Safari - ami-4bd6dc2f  
    Linux (Chrome Only) - ami-b7b7a2d3
    
#### 5.2.9 eu-central (Frankfurt)
    IE9/Chrome/Firefox/Safari - ami-879c85eb  
    IE10/Chrome/Firefox/Safari - ami-ec9b8280  
    IE11/Chrome/Firefox/Safari - ami-87f2ebeb  
    Linux (Chrome Only) - ami-6534e30a
    
#### 5.2.10 ap-northeast-1 (Tokyo)
    IE9/Chrome/Firefox/Safari - ami-4ed6e820  
    IE10/Chrome/Firefox/Safari - ami-ebd3ed85  
    IE11/Chrome/Firefox/Safari - ami-2f221c41  
    Linux (Chrome Only) - ami-31154856
    
#### 5.2.11 ap-northeast-2 (Seoul)
    IE9/Chrome/Firefox/Safari - ami-b2e12fdc  
    IE10/Chrome/Firefox/Safari - ami-76e12f18  
    IE11/Chrome/Firefox/Safari - ami-15e52b7b  
    Linux (Chrome Only) - ami-e9ac7f87
    
#### 5.2.12 ap-southeast-1 (Singapore)
    IE9/Chrome/Firefox/Safari - ami-f87ab69b  
    IE10/Chrome/Firefox/Safari - ami-ce78b4ad  
    IE11/Chrome/Firefox/Safari - ami-3e55995d  
    Linux (Chrome Only) - ami-f3cd7f90
    
#### 5.2.13 ap-southeast-2 (Sydney)
    IE9/Chrome/Firefox/Safari - ami-306c4853  
    IE10/Chrome/Firefox/Safari - ami-25644046  
    IE11/Chrome/Firefox/Safari - ami-e88eab8b  
    Linux (Chrome Only) - ami-f72f2294
    
#### 5.2.14 ap-south-1 (Mumbai)
    IE9/Chrome/Firefox/Safari - ami-7a86ec15  
    IE10/Chrome/Firefox/Safari - ami-bf80ead0  
    IE11/Chrome/Firefox/Safari - ami-d498f2bb  
    Linux (Chrome Only) - ami-b787f7d8
    
#### 5.2.15 sa-east (Sao Paulo)
    IE9/Chrome/Firefox/Safari - ami-79c54515  
    IE10/Chrome/Firefox/Safari - ami-7cc54510  
    IE11/Chrome/Firefox/Safari - ami-203abb4c  
    Linux (Chrome Only) - ami-38eb8a54
    
## 六、Updating Test Agents
The test agents will automatically update their code from the server if there are update files in place (in /work/update on the server).  Each update consists of a zip file (the actual updata) and an ini file that contains some meta-data about the update (most importantly, the software version).  There are separate updates for the IE agent (update.zip/update.ini) and the Chrome/Firefox agent (wptupdate.zip/wptupdate.ini).

Each new release includes updated agent binaries but sometimes it is helpful or necessary to update the agents in between releases if you need a bug fix or functionality that has been made available on the public instance but that hasn't been packaged up in a new release yet.  In this case you can download the update from the public instance of WebPagetest and deploy it on your private instance (the agents are backwards compatible so you do not need to update the web code unless you need updated functionality there).  

With WebPageTest 2.18 or later, the server can automatically update to the latest version as it is released.  In settings/settings.ini on the server, add:

    agentUpdate=http://cdn.webpagetest.org/

To download and update manually:
+ Download the agent updates:
    + http://www.webpagetest.org/work/update/update.zip
    + http://www.webpagetest.org/work/update/wptupdate.zip
+ Extract the ini file from within the zip files (update.ini and wptupdate.ini)
+ Copy the zip files to the /work/update folder on your server (you may want to back up the existing agent updates so you can roll back if necessary)
+ Copy the ini files to the /work/update folder on your server

After uploading the updates, each agent will automatically download and install the update before running their next test so you can be guaranteed that the update will be deployed before any more testing occurs.

## 七、Troubleshooting
### 7.1 Web Server
+ Waiting at the front of the queue
    + Commonly the issue is in your locations.ini. Double check that your location settings match that of which your agent is pulling the server with. Also note if you're using a key, do they match. You can check your Apache access logs for incoming requests being made by your test agent. 
+ The test completed but there were no successful results
    + If you're using a 64-bit Windows client you will be unable to perform traffic shaping (using dummynet.) In your locations.ini, add connectivity=LAN to the test location.
+ Waterfall charts are missing
    + Check if the GD library is installed. The GD library is used for drawing the waterfalls and generating thumbnail images.
    + Look to see if php-zip, php5-zip or a similar zip library is installed. With some default PHP distributions the library is not present.
+ Screen captures are black
    + When disconnecting from RDP, try rebooting the instance versus disconnecting from the RDP client. RDP locks up the desktop when you disconnect which will cause the screen shots and video to break.
+ Error message like this in /var/log/apache2/error.log:
    + [Mon Apr 30 10:18:14 2012] [error] [client 1.2.3.4] PHP Warning: POST Content-Length of 22689689 bytes exceeds the limit of 8388608 bytes in Unknown on line 0
    + PHP enforces a limit on the size of uploaded files, and an agent is uploading something larger than this limit. Change upload_max_filesize and post_max_filesize to larger values in php.ini.
    
#### 7.1.1 Test Agent
Low disk space
+ WebPagetest doesn't maintain any temporary files but sometimes Windows itself leaves stuff lying around and the disks can fill up. When that happens there are a few common things you can do to clean it up:
    + You can delete everything in "C:\Windows\SoftwareDistribution\Download". Windows keeps the full installer for every software update it installs and it doesn't need them after the install.
    + Try right-clicking on the c drive -> properties -> Disk Cleanup. There might be some crash reports it can cleanup
+ If that doesn't free enough space there are a few more things you can do:
    + Use windirstat to see what is taking up the disk space
    + It's possible the IE temporary Internet files got corrupt and is growing out of control. CCleaner can help fix that sometimes
    + Make sure hibernation is disabled (no hiberfile.sys on the c drive)
    + Worst case, you can disable the swap file to get back a gig or two