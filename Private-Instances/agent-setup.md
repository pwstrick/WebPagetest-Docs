# 代理安装

## 一、要求
你需要一个主机来运行代理。我们为每个设备创建一个代理。主机可以运行多个代理。  
任何支持NodeJS和adb的操作系统都应该可以正常工作，但是Linux和Windows是唯一可以进行定期测试的操作系统。 如果你的主机只有USB 3，你可能需要使用Windows（至少截至2014年2月，Linux上的USB 3驱动程序非常不稳定）。 如果你的主机作为USB 2.0端口，那么Linux或Windows是一个好的选择。  
对于Android，您将需要一个运行KitKat（4.4）或更高版本的手机，可以植根（Nexus设备和Motorola G的工作良好）。  
我们假设你使用的是Linux和Nexus4。

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
## 三、准备主机
### 3.1 Android-specific Host Configuration
### 3.2 iOS-specific Host Configuration (work in progress)
## 四、Connect and Verify the configuration
### 4.1 Android
## 五、Configure the locations.ini on the WPT server
## 六、Start the agent
## 七、Advanced features:
### 7.1 WebDriver Scripts
### 7.2 Android tcpdump
### 7.3 Traffic shaping
### 7.4 Video capture (non-android)
### 7.5 Run as per-device user