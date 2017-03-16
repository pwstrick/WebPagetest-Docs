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
#### 4.2.1 Headless Servers (including Google Compute Engine)
### 4.3 Mobile
## 五、EC2 Test Agents
### 5.1 Configuration
#### 5.1.1 Parameters
#### 5.1.2 Example User Data string
#### 5.1.3 Sample locations.ini
### 5.2 AMI Images
#### 5.2.1 Finding the Images (aka EC2 AMI search sucks)
#### 5.2.2 us-east (Virginia)
#### 5.2.3 us-east-2 (Ohio)
#### 5.2.4 us-west (California)
#### 5.2.5 us-west-2 (Oregon)
#### 5.2.6 ca-central-1 (Canada Central)
#### 5.2.7 eu-west-1 (Ireland)
#### 5.2.8 eu-west-2 (London)
#### 5.2.9 eu-central (Frankfurt)
#### 5.2.10 ap-northeast-1 (Tokyo)
#### 5.2.11 ap-northeast-2 (Seoul)
#### 5.2.12 ap-southeast-1 (Singapore)
#### 5.2.13 ap-southeast-2 (Sydney)
#### 5.2.14 ap-south-1 (Mumbai)
#### 5.2.15 sa-east (Sao Paulo)
## 六、Updating Test Agents
## 七、Troubleshooting
### 7.1 Web Server
#### 7.1.1 Test Agent