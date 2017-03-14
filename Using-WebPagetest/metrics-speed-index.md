#首屏展现平均值
&emsp;&emsp;`Speed Index`是显示页面的可见部分的平均时间。它以毫秒为单位表示，会受视图端口大小的影响。

&emsp;&emsp;`Speed Index`已于2012年4月添加到WebPagetest，衡量页面内容填充的速度（越低越好）。它特别适用于比较不同页面之间的差别（在优化之前/之后，我的网站与竞争对手等），与其它指标（`Load Time`，`Start Render`等）结合使用，可以更好地了解网站的性能 。

##一、问题
&emsp;&emsp;过去，我们依赖里程碑计时来确定网页速度的快慢。其中最常见的是浏览器到达主文档（document.onload）的加载事件之前的时间。这个时间很容易在实验室环境和现实世界中测量到。不幸的是，它不是一个非常好的实际用户的体验指标。随着页面的增长，内容的增多（超过当前屏幕的内容），这个加载事件将会被延迟执行。我们引入了更多的里程碑，尝试更好的表示时间，第一次渲染（First Paint）时间、 DOM内容准备（DOM Content Ready）时间等，但都有根本的缺陷，它们只测量单个点，并不能传达实际用户的体验。

##二、引入Speed Index
&emsp;&emsp;`Speed Index`采用可视页面加载的视觉进度，计算内容绘制速度的总分。为此，首先需要能够计算在页面加载期间，各个时间点“完成”页面是如何“完成”的。在WebPagetest中，通过捕获在浏览器中加载页面的视频并检查每个视频帧（当前实现中为每秒10帧，并且仅用于启用视频捕获的测试）来完成的，这个算法在下面有描述，但现在假设我们可以为每个视频帧分配一个完整的百分比（在每个帧下显示的数字）。翻译的比较生硬，下面是原文：
>The speed index takes the visual progress of the visible page loading and computes an overall score for how quickly the content painted.  To do this, first it needs to be able to calculate how "complete" the page is at various points in time during the page load.  In WebPagetest this is done by capturing a video of the page loading in the browser and inspecting each video frame (10 frames per second in the current implementation and only works for tests where video capture is enabled).  The current algorithm for calculating the completeness of each frame is described below, but for now assume we can assign each video frame a % complete (numbers displayed under each frame):

![](/assets/img/using/metrics/compare_progress.png)

如果我们绘制一个页面的完整性与时间的折线图，我们将最终得到如下所示的东西：

![](/assets/img/using/metrics/chart-line-small.png)

然后，我们可以通过计算曲线下的面积将进度转换为数字：

![](/assets/img/using/metrics/chart-progress-a-small.png)
![](/assets/img/using/metrics/chart-progress-b-small.png)

如果页面在达到视觉上完成后旋转10秒，分数将继续增加。涂色的是渲染部分，面积越小，页面载入速度越快。原文如下：
>This would be great except for one little detail, it is unbounded.  If a page spins for 10 seconds after reaching visually complete the score would keep increasing.  Using the "area above the graph" and calculating the unrendered portion of the page over time instead gives us a nicely bounded area that ends when the page is 100% complete and approaches 0 as the page gets faster:

![](/assets/img/using/metrics/chart-index-a-small.png)
![](/assets/img/using/metrics/chart-index-b-small.png)
![](/assets/img/using/metrics/speedindexformula.png)

`Speed Index`是以“ms”为单位计算的“曲线上方区域”，在视觉完成范围内使用0.0-1.0。间隔分数计算公式如下：
> IntervalScore = Interval * (1.0 - (Completeness / 100)) 

Completeness是该帧的完成百分比，Interval是该视频帧以毫秒为单位的经过时间（这里为100）。总分是各个间隔的总和：SUM（IntervalScore），原文如下：
>The Speed Index is the "area above the curve" calculated in ms and using 0.0-1.0 for the range of visually complete.  The calculation looks at each 0.1s interval and calculates IntervalScore = Interval * (1.0 - (Completeness/100)) where Completeness is the % Visually complete for that frame and Interval is the elapsed time for that video frame in ms (100 in this case).  The overall score is just a sum of the individual intervals: SUM(IntervalScore)

为了比较，下面是两页的视频帧（上面是“A”，下面是“B”）：

![](/assets/img/using/metrics/compare_trimmed.png)

##三、测量视觉进展(Measuring Visual Progress)
&emsp;&emsp;我用手挥舞着如何计算每个视频帧的“完整性（completeness）” ，`Speed Index`本身的计算是独立的技术，用于确定完整性（可以与不同的计算完整性的方法一起使用）。目前使用两种方法：
###1. 视频捕获的可视进度(Visual Progress from Video Capture)
&emsp;&emsp;简单的方法将图像的每个像素与最终图像比较，然后计算每个帧匹配的像素百分比。这种方法的主要问题是，网页是流动的，像广告加载这样的东西可以导致页面的其余部分移动。在像素比较模式下，屏幕上的每个像素的移动都会改变比对结果，即使实际内容只是向下移动一个像素。

&emsp;&emsp;我们确定的技术是采取图像颜色的直方图（红色，绿色和蓝色各一个），只看一下页面上颜色的整体分布。我们计算起始直方图（对于第一个视频帧）和结束直方图（最后一个视频帧）之间的差异，并使用该差异作为基线。将视频中的每个帧的直方图与第一直方图的差异与基线进行比较，以确定该视频帧是如何“完成”的。在某些情况下，可能准确，但大部分情况下，这种方法还是值得做的。

&emsp;&emsp;这是原始机制，用于计算`Speed Index`的视觉进度，但在某些情况下，它有问题（视频播放页面，幻灯片动画或大的插页式广告）。它对结束状态非常敏感，根据最终图像计算进度。它也只能在实验室中测量，并且依赖于视频捕获。

###2. 从绘画事件的可视进展(Visual Progress from Paint Events)
&emsp;&emsp;最近，我们（成功地）试验了Webkit开发者工具时间线（也可以通过扩展和远程调试协议）公开的Paint事件。它适用于所有最新的基于webkit的浏览器，包括桌面和移动和所有平台。它也非常轻量级，不需要捕获视频。它对给定浏览器中的渲染器实现有些敏感，因此它不能用于比较不同浏览器的性能。

为了获得有用的数据，它需要一个公平的过滤和加权。

我们使用的特定算法来计算从dev工具绘制rects的`Speed Index`是：

+ 在基于Webkit的浏览器的情况下，我们收集时间线数据，其中包括绘制矩形以及其他有用的事件。
+ 我们过滤掉在接收到第一个响应数据之后，第一个布局之前的任何绘制事件。
	+ ResourceReceiveResponse -> Layout -> Paint事件.
	+ 这是因为浏览器在实际处理任何数据之前会执行多个绘制事件。
+ 我们将所有绘画事件按他们正在更新的矩形分组（帧ID，x，y，width，height）。
+ 我们认为绘制的最大`Paint`矩形是“全屏”矩形。
+ 每个矩形贡献一个总分的总分。给定矩形的可用点是矩形的面积（width x height）。
+ 全屏绘制（最大矩形的任何`Paint`事件）计为50％，以便它们仍然有贡献但不支配进度。
+ 总体总和是每个矩形的点的总和。
+ 给定矩形的点在绘制该矩形的所有`Paint`事件上均匀分布。	
    + 一个带有单个`Paint`事件的矩形将贡献全部区域。
	+ 具有4个`Paint`事件的矩形将为每个`Paint`事件贡献其区域的25％。
+ 任何给定`Paint`事件的结束时间用于该`Paint`事件的时间。
+ 通过将每个`Paint`事件的贡献添加到运行总计中，接近总体（100％）来计算视觉进展。
>+ In the case of Webkit-based browsers, we collect the timeline data which includes paint rects as well as other useful events.
 + We filter out any paint events that occur before the first layout that happens after the first response data is received.
    + ResourceReceiveResponse -> Layout -> Paint events.
