# 快速入门指南
WebPagetest的核心是用于测量和分析网页的性能。有很多选项，看着很吓人，但其实做快速测试是很简单的。
本指南将引导你提交测试和结果解释。

## 一、运行性能测试(Running a Performance Test)
### 1.1 输入网页网址(Enter The Page URL)

你需要做的第一件事是决定一个页面来测试。大多数人从他们的网站的主页开始（但不要忽视人们访问的其他页面）。确定要测试的页面后，转到WebPagetest并为其指定要测试的页面的URL：

![](/assets/img/using/guide/url.png)
### 1.2 选择位置(Select a Location)
接下来，应该决定从哪里运行测试。WebPagetest具有位于世界各地的测试机器，你应该从接近用户访问的位置进行测试，从列表中选择一个位置，或者单击`Select from Map`按钮，从地图视图中选择一个位置（只需单击气球，然后确定）。如果将指针放在气泡上，它们将显示一条消息，提示位置信息：

![](/assets/img/using/guide/map.png)
### 1.3 选择浏览器(Select a Browser)
最后，需要决定使用什么浏览器进行测试。**不同的位置支持不同的浏览器**，如果给定的位置没有正在寻找的浏览器，可以尝试不同的位置。 Dulles，VA USA位置支持WebPagetest工作的所有浏览器（IE 6,7,8和9）。现在忽略“dynaTrace”浏览器，这些用于更高级的分析。我们通常建议使用IE7进行初始测试，因为它几乎是最糟糕的情况，并且更容易看到很多问题，所以如果你不知道什么浏览器，开始只需使用IE7。

![](/assets/img/using/guide/browser.png)
### 1.4 提交测试(Submit the Test)
一切配置完成后，点击`START TEST`按钮，请求将发送到测试位置进行测试。测试可能需要一段时间才能运行，具体取决于有多少次测试（在测试之前至少有一分钟的测试时间，但是它的时间甚至更长）。一旦测试完成，你将得到结果。

## 二、解释结果(Interpreting the Results)
第一次看到结果信息可能有点吓人，信息量有点大，但首先可以先查看一些关键信息。

### 2.1 优化等级(Optimization Grades)
在结果页面的顶部是一组最关键的性能优化等级。涵盖了适用于所有网站的基本优化，任何不是A或B的都需要进行进一步的优化。

![](/assets/img/using/guide/grades.png)

| 字母等级 | 占比  |
| ------------ |:---------------|
| A     | 90%+ |
| B     | 80% ~ 89%        |
| C     | 70% ~ 79%        |
| D     | 60% ~ 69%        |
| F     | 0% ~ 59%        |

#### 2.1.1 阻塞时间（First Byte Time）
首字节时间是指浏览器收到HTML内容的第一个字节时间，包括DNS查找、TCP连接、SSL协商（如果是HTTPS请求）和TTFB（Time To First Byte）。  
关于First Byte Time和TTFB的区别在[Metrics](/Using-WebPagetest/metrics.md)一节中做了简单分析。

    预期首字节 = RTT * 3 + SSL
    比值 = 100 - (实际观测首字节 - 预期首字节) / 10
其中`RTT`的指往返通信时间。更多网络术语可以参考整理的[网络协议](http://www.cnblogs.com/strick/p/6262284.html)

#### 2.1.2 长连接已启动(Keep-alive Enabled)
请求网页上的内容（图像、JavaScript、CSS、Flash等）需要与Web服务器建立连接。每次都重新连接会耗费一些时间，所以最好复用连接，`keep-alive`实现了这个方法。默认情况下，在配置中已启用，而且是HTTP 1.1标准的一部分，但有时它们将被破坏（可能是无意的）。启用`keep-alive`通常只是服务器上的配置更改，不需要对页面本身进行任何更改，通常可以将加载页面的时间减少40-50％。计算公式如下：  

    比值 = 实际复用连接数 / 预期复用连接数

#### 2.1.3 压缩文本(Compress Text)
除了图片或视频，剩余的都是某种类型的文本（html，javascript，css等）。HTTP提供了一种以压缩形式传输文件的方法。启用文本资源压缩通常只是服务器配置更改，不需要对页面本身进行任何更改，提高性能降低服务内容的成本（通过减少传输的数据量）。由于文本资源通常在页面的开头（javascript和css）下载，因此，提供文本资源的快慢很影响用户体验。计算公式如下：  

    比值 = 资源压缩后的大小 / 实际资源的大小

#### 2.1.4 压缩图片(Compress Images)
图像压缩检查仅查看照片图像（JPEG文件），确保质量不会设置得太高。JPEG图像可以在不降低视觉质量的情况下压缩。我们可以在Photoshop的“网络存储”模式中，使用一种质量级别为“50”的压缩图像的标准。在视觉质量不是很差的情况下，尽量多的压缩图像。在照片中经常包含其他数据，特别是如果照片来自数码相机（包含关于相机，镜头，位置，缩略图的信息），其中一些应该在被发布到网络之前就移除（小心保留任何版权信息）。计算公式如下：  

    比值 = 图片压缩后的尺寸 / 图片实际的尺寸

#### 2.1.5 缓存静态内容(Cache Static Content)
静态内容是网页上不经常更改的内容（图片，javascript，css）。可以配置它们，以便用户的浏览器将它们缓存起来，如果用户回到页面（或访问使用相同文件的其他页面），他们可以使用已经拥有的副本，而不是重新请求文件Web服务器。在用户浏览器中成功缓存静态内容可以显著提高重复访问的性能（高达80+％，具体优化率取决于页面），并能减少Web服务器上的负载。有时可能很难实现缓存而不改变页面，所以不要盲目地启用它（你需要能够更改希望改变的任何文件的文件名）。

#### 2.1.6 合并JS/CSS文件(Combine JS/CSS Files)
提高性能通常意味着减少对内容的请求数，最简单（最重要的）方法之一是减少在页面开头加载的css和javascript文件数量（在<head> 中会阻塞页面显示）。一个简单的实现方法是将JavaScript和CSS分别合并到一个文件中（CSS最好在JavaScript之前加载）。减少用户从屏幕上看到东西的等待时间，对用户体验有巨大的影响。计算方式：  

    比值 = 过期资源数得分 / 静态资源总数

这个评级需要一个缓存生命周期评分系统，需要和资源响应头中的参数对应：`Cache-Control:max-age`和`Expires Header`。如果一个静态资源的生命周期超过一周，就100分，超过一小时，最多50分，以上情况之外都是0分。

#### 2.1.7 使用CDN(Use of CDN)
每个内容的请求都是从用户的浏览器到Web服务器，再返回。随着距离越来越远，这样一个往返可能消耗很多时间（页面上的请求越多，消耗的时间越多），把你的服务器建立在离用户比较近的地方就能解决这个问题，这正是内容分发网络（CDN）的作用。他们在世界各地都有靠近用户的服务器，从靠近用户的服务器提供网站的静态内容。使用CDN没有意义的唯一情况是如果网站的所有用户都已经接近Web服务器（例如社区网站）。计算方式：

    比值 = 通过CDN服务获取的静态资源数 / 静态资源总数

### 2.2 高级度量(High-level Metrics)
结果页顶部的数据表提供了有关已加载页面的一些高级信息：

![](/assets/img/using/guide/WebPagetest_example.jpeg)
#### 2.2.1 首次视图(First View)
首次视图的测试，将会把浏览器的缓存和Cookie清除，表示访问者第一次访问该网页，将体验到的情况。

#### 2.2.2 重复视图(Repeat View)
重复视图会在首次视图测试后立即执行，不会清除任何内容。浏览器窗口在`First View`测试后关闭，然后启动新浏览器以执行`Repeat View`测试。重复视图测试模拟的是用户离开页面后，马上再进入此页面的场景。

#### 2.2.3 文档加载完毕(Document Complete)
从初始化请求，到加载所有静态内容（图片、CSS、JavaScript等），但可能不包括由JavaScript执行触发的内容，可以理解为开始执行`window.onload`。原文如下：

>The metrics grouped together under the Document Complete heading are the metrics collected up until the browser considered the page loaded (onLoad event for those familiar with the javascript events).  This usually happens after all of the images content have loaded but may not include content that is triggered by javascript execution.

#### 2.2.4 页面所有元素加载时间(Fully Loaded)
从初始化请求，到Document Complete后，2秒内没有网络活动的时间，这包括在主网页加载后由JavaScript触发的任何活动。原文如下：
>The metrics grouped together under the Fully Loaded heading are the metrics collected up until there was 2 seconds of no network activity after Document Complete.  This will usually include any activity that is triggered by javascript after the main page loads.

#### 2.2.5 整页加载时间(Load Time)
与`Document Complete`中的时间属性相同。原文如下：
>The Load Time is the time from when the user started navigating to the page until the Document Complete event (usually when all of the page content has loaded).

#### 2.2.6 第一个字节加载时间(First Byte)
这个时间表示从初始化请求到服务器响应后，接收到第一个字节的时间。此时的大部分时间通常称为“后端时间”，服务器为用户构建页面的时间量。原文如下：
>The First Byte time is the time from when the user started navigating to the page until the first bit of the server response arrived.  The bulk of this time is usually referred to the "back-end time" and is the amount of time the server spent building the page for the user.

#### 2.2.7 页面渲染时间(Start Render)
渲染时间表示屏幕上显示东西的第一个时间点，在这个时间点之前，用户看到的是一个空白页。这并不表示用户看到了内容，可能只是一个简单的背景色，但这是用户开始看到内容的第一个指标，`我理解这个为白屏时间`。原文如下：
>The Start Render time is the first point in time that something was displayed to the screen.  Before this point in time the user was staring at a blank page.  This does not necessarily mean the user saw the page content, it could just be something as simple as a background color but it is the first indication of something happening for the user.

#### 2.2.8 DOM元素数量(DOM Elements)
在测试结束时测试页面上的DOM元素的计数。

#### 2.2.10 HTTP请求数(Requests)
浏览器针对页面上的内容（图片，javascript，css等）发出的请求数。

#### 2.2.11 传输的字节量(Bytes In)
浏览器加载页面下载的数据量。它通常也被称为“页面大小”。

---
以前曾写过《[前端页面性能参数搜集](http://www.cnblogs.com/strick/p/5750022.html)》的文章，大家可以探讨一下。