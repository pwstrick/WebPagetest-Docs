# 移动测试

对于移动测试，有几个网络连接选项：
+ 实际移动网络具有实际载体
+ WiFi没有流量整形
+ WiFi具有固定的流量整形配置文件
+ WiFi具有每个测试流量整形

[流量整形](http://baike.baidu.com/link?url=tl7WeKr3B-0Vr0NkUUNY1iqky_R1lsFOYRb2WwGq3dY6VMrkOecrrhKL3-fw4gP8SgMNQqgLm8x8XUU-ZtElqNHJshaVCndxJYQJLcy03epoEDFZvIx6zwHnjeAmm9gF)(traffic shaping)典型作用是限制流出某一网络的某一连接的流量与突发，使这类报文以比较均匀的速度向外发送。  
移动测试网络对于WebPagetest的公共实例来说是至关重要的（至少在2014年的这篇文章中）：

![](/assets/img/system/WebPagetest_Mobile_Network.png)

不同的配置建立在彼此之上：

## 一、设备
&emsp;&emsp;使用新的Node.js代理进行测试需要运行4.4（Kit Kat）的Android设备或更高版本，iOS设备运行最新版本的iOS（更多的iOS文档和更新的支持即将推出）。在这两种情况下，设备需要刷机（在iOS的情况下越狱）才能删除浏览器缓存。对于Android，Nexus和Motorola设备的工作效果很好，并提供广泛的功能。需要单独的设备来测试人像与景观（主要是平板电脑测试的问题）。

## 二、实际移动网络具有实际载体
&emsp;&emsp;最简单的设置是在运营商网络上运行设备。在这种情况下，您只需要手机和有线主机来控制它们。可以从单个主机控制多个设备（OS将允许USB连接）。任何支持adb和Node.js的操作系统都可以通过Windows和Linux进行测试。  
&emsp;&emsp;如果主机有USB 3控制器，并且想要使用Linux，则需要确保它是一个比较新的内核，因为早期版本中的USB 3支持是非常差。  
&emsp;&emsp;对于Windows，还有一个支持应用程序[adbwatch](https://github.com/WPO-Foundation/adbwatch/releases)来监视adb，如果它死机（有时会停止响应），它将自动重新启动，允许系统运行，而不需要人为干预。  
&emsp;&emsp;公共实例目前有10台设备连接到运行Windows 7的英特尔i5 NUC，具有管理代理实例并保持adb运行的adbwatch。将NUC作为无头服务器（对于不支持AMT的较新的haswell型号）运行的一个提示是使用Windows附带的VNC和Microsoft视频驱动程序，而不是Intel。如果使用英特尔驱动程序，显示屏将被禁用，VNC将无法正常工作。  
&emsp;&emsp;测试结果和代理服务器通信的数据流全部发生在系统主机上，因此唯一使用的数据连接将是实际的测试数据（以及任何软件更新）。

## 三、WiFi没有流量整形
通过WiFi连接进行测试可以更好地控制结果的稳定性，通常会产生比运营商网络上测试更加一致的结果。运行稳定WiFi测试连接的主要问题是：
+ **使用稳定的接入点**：大多数消费者WiFi设备是可怕的，一直存在，并且不能长时间保持连接。业务设备往往更加可靠，但我发现，苹果Air Port设备的工作非常好，只能使用它们。它们是坚如磐石的，永远不会`冻结/挂起/重新启动`，允许在接入点模式下运行，同时支持2.4和5GHz频段。
+ **使用清晰的WiFi频率**：在办公室或城市环境中，这可能是要解决的最困难的问题。我使用inssider找到一个清晰的非重叠频段用于测试网络。对于公共实例，我很幸运，因为没有邻居网络干扰，所以我选择了全方位。
+ **不要超载WiFi网络**: 将单个通道上的设备太多或使用相同的网络用于其他流量可能会导致争用，并可能限制实际带宽。使用可以与802.11n或802.11ac通话的设备变得不那么简单，但如果正在部署大量设备，那么值得牢记。解决这个问题的唯一方法是跨多个网络分流流量，将设备和接入点（将它们放在法拉第笼中）或使用rndis有线网络而不是WiFi绝缘。

测试未定义的WiFi连接的主要缺点是以ISP的完全连接速度运行，并且可能与最终用户的体验不符。

## 四、WiFi具有固定的流量整形配置文件
&emsp;&emsp;将流量整形添加到WiFi测试配置，允许测试用户连接特性，保持常规WiFi测试的一致性。流量整形必须在接入点的远端完成，最好使用将网络连接从接入点桥接到有线网络的其余部分的FreeBSD机器（还有一个dummynet的linux端口，但FreeBSD的实现在我们的测试中已经更加一致了）。  
&emsp;&emsp;使用固定流量整形配置文件，必须选择一个配置文件，并将其用于给定设备（或所有设备）的所有流量。设置dummynet配置时，务必记住给每个设备自己的“管道”，以使它们不共享虚拟连接。  
&emsp;&emsp;就硬件而言，任何支持FreeBSD并具有2+网络接口的机器都可以工作。我喜欢Supermicro Atom服务器，因为它们便宜，超低功耗，支持远程管理。  
以下是公共实例使用的ipfw配置：
```bash
# 3G配置文件 - 1.6Mbps下行，768Kbps上行，300ms RTT
# src-ip和dst-ip掩码确保每个客户端都获得自己的管道
ipfw pipe 1 config bw 1600Kbit/s delay 150ms noerror mask dst-ip 0xffffffff
ipfw pipe 2 config bw 768Kbit/s delay 150ms noerror mask src-ip 0xffffffff

# em1是连接到接入点的网络接口。
# 数据“transmitted”到接入点是设备上的“in”数据。
ipfw add pipe 1 ip from any to any out xmit em1
ipfw add pipe 2 ip from any to any out recv em1
```

## 五、WiFi具有每个测试流量整形
&emsp;&emsp;不用为所有设备使用固定的连接配置文件，可以使测试代理程序在每个测试的基础上进行配置。Node.js代理可以调用代理上的ipfw shell脚本，其中包含要配置的测试代理和连接配置文件的信息。[github中有示例代码](https://github.com/WPO-Foundation/webpagetest/blob/master/agent/js/ipfw_config) 动态创建管道并分配相应的配置文件。  
&emsp;&emsp;对于公共实例，静态配置多个管道，每个设备一对，然后当脚本配置整形时，它将只设置与该设备相关联的管道参数。这就是桌面代理的工作原理，并避免了任何边缘情况，如果某些事情死机，那么在配置中会留下规则。一旦设置和工作，我将提供在公共实例上实现的脚本和配置。

## 六、rndis替代WiFi
&emsp;&emsp;配置更大的实验室的一个更困难的问题是管理WiFi网络。可以反转系统的设备，使其网络流量通过USB连接路由到有线主机。这可以提供更一致的结果，但是需要注意不要超载USB总线（这也会传输其他系留设备的视频和测试结果），并且可靠性还不如WiFi（设备往往脱机）。