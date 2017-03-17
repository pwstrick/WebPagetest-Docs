# 代理安装

## 一、要求
&emsp;&emsp;你需要一个主机来运行代理。我们为每个设备创建一个代理。主机可以运行多个代理。  
&emsp;&emsp;任何支持NodeJS和adb的操作系统都应该可以正常工作，但是Linux和Windows是唯一可以进行定期测试的操作系统。 如果你的主机只有USB 3，你可能需要使用Windows（至少截至2014年2月，Linux上的USB 3驱动程序非常不稳定）。 如果你的主机作为USB 2.0端口，那么Linux或Windows是一个好的选择。  
&emsp;&emsp;对于Android，您将需要一个运行KitKat（4.4）或更高版本的手机，可以植根（Nexus设备和Motorola G的工作良好）。  
&emsp;&emsp;我们假设你使用的是Linux和Nexus4。

## 二、准备设备
### 2.1 Android
+ 根设备（首先执行此操作，因为它会清除所有设置）
    + Nexus设备（4，5，7） - [cf-auto-root](https://autoroot.chainfire.eu/)（最好是手动“adb reboot bootloader”和“fastboot oem unlock”）
    + 摩托罗拉G（[解锁引导加载程序](https://motorola-global-portal.custhelp.com/app/standalone/bootloader/unlock-your-device-a/action/auth)然后使用[超级引导](http://www.modaco.com/topic/366771-root-your-moto-g-option-1-superboot/)）
+ 进入SuperSU设置（如果已安装）并禁用通知并将默认访问权限设置为grant
+ 从Play商店安装Chrome，Chrome测试版和Chrome开发者（请确保Chrome更新到最新版本）
    + 启动两者并接受服务条款，并关闭任何“帮助使Chrome更好”对话框
    + 确保Google Play已配置为自动安装更新
+ 停用滑动即可解锁（可选，在安全设置中）
+ 激活开发人员模式（点击操作系统版本7次）
+ 启用USB调试（在开发人员选项中）
+ 停用屏幕睡眠（在开发者选项中）
+ 禁用屏幕自动旋转（除非设置横向测试设备）
    + 大多数发行版都有快速设置锁定屏幕方向，否则可能在“显示”菜单下作为“当设备旋转时”设置，选择保持纵向模式。
+ 将屏幕亮度降至最低（减少热量和电源使用）
+ 静音音量（可选）
+ 将手机设置为飞行模式并启用WiFi（可选，仅当未通过蜂窝连接进行测试时）
+ 如果在Android 5.0或更高版本（Lollipop）上停用与近期应用的标签集成
    + 打开Chrome的每个实例（stable，beta，dev）
    + 打开设置
    + “合并标签页和应用程序” - 关闭
### 2.2 iOS (work in progress)
+ 越狱的设备（包括Cydia）
+ 安装OpenSSH
    + 在绑定的主机上创建ssh密钥并另存为`~/.ssh/id_dsa_ios`
    + 将ssh公钥添加到`~/.ssh/authorized_keys`
+ 安装tcpdump
+ 锁定设备旋转（如果需要）
+ 启用Web检查器（设置 - > Safari - >高级）
+ 用于设备管理的有用应用程序（来自cydia）：
    + “veency” - VNC服务器远程触发滑动解锁，方便重新启动设备
    + “核心实用程序（Core Utilities）” - 包括检查存储使用的
    + “支持不支持的附件8” - 防止“此附件可能不支持”消息从间歇性出现（请确保在设置中启用它）

## 三、准备主机
Raspberry Pi设备是运行Android设备的主机。[此处](https://github.com/WPO-Foundation/webpagetest/blob/master/docs/Private%20Instances/MobileAgentRaspberryPi.md)提供了描述如何配置它们的文档。
+ 安装NodeJS（`shell -v`从shell /命令行验证）
    + Ubuntu：`sudo apt-get install nodejs`然后`sudo ln -s /usr/bin/nodejs /usr/sbin/node`
    + Windows：安装Windows版本的node.js并将其添加到你的路径（作为安装选项或手动）
    + OSX：`brew install node`
+ 安装ImageMagick（屏幕截图和视频处理所必需的，从shell /命令行`convert -version`验证）
    + Ubuntu：`sudo apt-get install imagemagick`
    + Windows：安装Windows版本的ImageMagick并将其添加到您的路径（作为安装选项或手动）
    + OSX：`brew install imagemagick`
+ 安装ffmpeg与x264支持（视频处理所必需）
    + OSX：`brew install ffmpeg`
+ 安装python 2.7和支持库
    + 安装Pillow库：`pip install pillow`
    + 安装ujson（用于更快的跟踪解析）：`pip install ujson`
+ 安装代理代码
    + 手动安装需要`agent/js`目录
    + 直接使用git（假设在`~/wpt`，但可以安装在任何地方）：
```bash
cd ~
git clone http://github.com/WPO-Foundation/webpagetest.git wpt
``` 
+ 连接手机

### 3.1 Android特定主机配置
+ 安装adb并确保它在你的路径（“adb设备”从shell /命令行来验证设备连接后）
    + Ubuntu：`sudo apt-get install android-tools-adb`
    + 否则，从[Android SDK](http://developer.android.com/sdk/index.html)
+ 配置[udev规则](https://developer.android.com/studio/run/device.html)（仅限Linux）

### 3.2 iOS特定主机配置（工作进行中）
+ 需要OSX Yosemite（10.10）或更高版本进行视频捕获
    + 截至目前，OSX只能从一个设备捕获视频。这就需要为每个iPhone使用专用的Mac，用于测试。
+ 需要在OSX上的XCode屏幕截图
    + 复制`/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport` 到 `wpt/agent/js/lib/ios/DeviceSupport`
    + 确保目录名称仅包含操作系统版本（无构建信息）
+ 安装`libimobiledevice` - `brew install libimobiledevice`
+ 安装`ios_webkit_debug_proxy` - `brew install ios_webkit_debug_proxy`

## 四、连接并验证配置
### 4.1 Android
+ 授权并验证与设备的adb连接
    + `adb devices` - 确保列出了设备（出现提示时，单击设备屏幕上的授权）
+ 验证根
    + adb shell su -c date
+ 验证网络是否已启动
```bash
adb shell netcfg | grep wlan
wlan0 UP 1.2.3.4/26  0x00001043 ac:47:e8:4b:3a:81

adb shell ping yahoo.com
```
+ 验证Chrome是否正常运行
```bash
adb shell am start \
    -n com.android.chrome/com.google.android.apps.chrome.Main \
    -d http://yahoo.com
```

## 五、在WPT服务器上配置locations.ini
修改WPT服务器设置以添加测试位置和浏览器，例如：
```bash
Test location: Example
Browser: Nexus4 - Chrome
```
通过编辑`settings/locations.ini`：
```bash
[locations]
1=Example

[Example]
1=Example_Nexus4
label="Example"

[Example_Nexus4]
browser=Nexus4 - Chrome,Nexus4 - Chrome Beta
label="Nexus 4"
type=nodejs,mobile
connectivity="WiFi"
```
要使用Play商店中的Chrome和Chrome测试版，浏览器名称需要分别以“ - Chrome”和“ - Chrome测试版”结尾。

## 六、启动代理
启动代理：
```bash
cd ~/wpt/agent/js
./wptdriver.sh \
  -m debug \
  --browser android:0088a434deadbeef \
  --serverUrl example.com \
  --location Example_Nexus4 \
  --processvideo yes
```
它应该打印：

    NODE_PATH=...
    node src/agent_main ...
    I 0913Z22:06:45.073 wpt_client.js:293 Client.requestNextJob_ : \
    Get work: http://example.com/work/getwork.php?location= Example_Nexus4&pc=0088a434deadbeef&f=json
    .....

它将每10秒轮询上述URL。  
要可选地保持代理运行，如果/当你注销，请包装上述命令为：
```bash
nohup ... &>0088a434deadbeef.log &
```
（可选）指定特定的Chrome浏览器包：
```bash
--chromePackage com.android.chrome
```
其他可用的命令行选项：
```bash
--alive <name>          // Touch a <name>.alive file while running (for integration with adbwatch.exe to auto-restart dead agents)
--apiKey <key>                    // Use the specified api key when polling for work
--checknet yes                    // Check for a valid assigned IP address before polling for work 
--chromePackage com.android.chrome    // Specify an explicit browser package 
--maxtemp <temp>                // Maximum allowed battery temp - i.e. --maxtemp 36.8
--name <friendly name>  // Use the specified friendly device name for reporting instead of the device ID
--processvideo yes      // Process video capture locally instead of on the server.  Requires python 2.7 in the path and visualmetrics.py in the agent directory (run "python visualmetrics.py -c" to validate config).
--rotate <degrees>      // Rotate the screen shots (useful for landscape testing)
--tcpdumpBinary <path>  // Path to an arm build of tcpdump to be installed on the agent dynamically
--trafficShaper <info>  // See details below in "Traffic Shaping"
--exitTests <test count> // Exits after running the specified number of tests (helpful to keep node's memory use under control)
```

## 七、高级功能
### 7.1 WebDriver脚本
在桌面浏览器上，将作业提交到`http://example.com`，例如：  
    Advanced Settings > Script > Enter Script：
```javascript
driver = new webdriver.Builder().build();
driver.get('http://www.google.com');
driver.findElement(webdriver.By.name('q')).sendKeys('webdriver');
driver.findElement(webdriver.By.name('btnG')).click();
driver.wait(function() {
    return driver.getTitle();
})
```

### 7.2 Android tcpdump
+ 从[omappedia.org](http://omappedia.org/wiki/USB_Sniffing_with_tcpdump)下载一个预编译的tcpdump二进制文件。
+ Gunzip并将二进制文件复制到`~/wpt/lib/tcpdump`
+ 重新运行wptdriver.sh与其他args：`--tcpdumpBinary ~/wpt/lib/tcpdump`
+ 当你提交作业时，通过以下方式启用tcpdump：
    + Advanced Settings > Advanced > Capture network packet trace (tcpdump)
+ 验证代理是否上传pcap。

### 7.3 流量整形
On-device traffic shaping:
    TODO Eg. Android [tc](https://www.cyberciti.biz/faq/linux-traffic-shaping-using-tc-to-control-http-traffic/) (requires on-device /proc/net/psched and /system/bin/tc).
    Can likely implement via the below "trafficShaper" script approach.
The easiest configuration is to assume that all traffic passes through the desktop, e.g:
    Add a second ethernet card to your desktop.
    Attach WiFi access point to this additional card.
    Verify that Linux enabled IP forwarding ([how-to](http://www.ducea.com/2006/08/01/how-to-enable-ip-forwarding-in-linux/))
    Configure your mobile device to use the WiFi access point.
    Verify that your device's WiFi traffic is routed through your desktop (via tcpdump/wireshark?)
If traffic is not routed through your desktop, create a local "trafficShaper" script that (e.g.) uses passwordless ssh to configure the remote switch.
[issue/147](https://github.com/WPO-Foundation/webpagetest/issues/147) adds support for user-defined "trafficShaper" script:
    Create a traffic shaper script, e.g. ~/wpt/shaper
    Run ./wptdriver.sh with "--trafficShaper ~/wpt/shaper" and optional "--trafficShaperArg anyString" (e.g. foo1.2.3.4).
    Modify the WPT Server's settings/locations.ini to remove the "connectivity=" line (to allow the job options).
    Submit job with Advanced Settings > Connection set to (e.g.) "56K Dialup-up", which the server's connectivity.ini defines as:
```bash
label="56K Dial-Up (49/30 Kbps 120ms RTT)"
bwIn=49000
bwOut=30000
latency=120
plr=0
```
    Before each run, the agent should call:
```bash
~/wpt/shaper -s 0088a434deadbeef start --arg foo1.2.3.4 --bwIn 49 --bwOut 30 --latency 120
```
    where "0" values are not passed (e.g. the above plr=0)
    The agent will check the script's stdout for an optional line matching "stop=VALUE" (e.g. "stop=bar42").
    Verify that pages load slowly due to your traffic shaper.
    After each run, the agent should call:
```bash
~/wpt/shaper -s 0088a434deadbeef stop --arg bar42
```

TODO rough ipfw notes:
    Install ipfw3, sudo chmod 7755 /sbin/ipfw, verify that ipfw3 list prints "65535 allow ip from any to any"
    Get the device IP from `adb shell netcfg | grep wlan" (e.g. 1.2.3.4), run ./wptdriver.sh with "--deviceAddr 1.2.3.4"
    Modify the WPT Server's settings/locations.ini to remove the "connectivity=" line.
    Submit job with Advanced Settings > Connection set to (e.g.) "56K Dialup-up".
    Verify that the agent prints the expected ipfw commands (e.g. `ipfw add ...`, `ipfw pipe ...`)
    Verify that pages load slowly due to traffic shaping.
    
### 7.4 视频捕获（非Android）
TODO rough notes:
    Create capture script, e.g. ~/wpt/video/capture
    Run ./wptdriver.sh with "--captureDir ~/wpt/video" and optional "--videoCard anyString" (e.g. to pass the card name to the script).
    Agent will call
```bash
~/wpt/video/capture -f pid_video.avi -s 0088a434deadbeef -t mako -d anyString -w
```
    where "mako" is the `adb shell getprop ro.product.device` codename for Nexus4.
    When the agent wants to stop the capture, it'll kill the above process, then read the "pid_video.avi".
    
### 7.5 按每个设备用户运行
Primarily used to kill any zombie processes, e.g. leftover video captures.
Rough notes:
    Create a user for this device, e.g. via:
```bash
sudo useradd -d /home/0088a434deadbeef -m 0088a434deadbeef
```
    Switch to this user (optionally via /etc/sudoers.d/wpt file and `sudo -u`)
    Run ./wptdriver.sh with "--killall 1"
    The agent will `killall -9` all pids owned by its user, except its own pid.  This `killall` ensures that there are no zombie processes between jobs, but note that it'll also kill any login shells, so make sure to run with nohup!