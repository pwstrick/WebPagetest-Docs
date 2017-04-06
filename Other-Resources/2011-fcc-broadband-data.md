# 2011 FCC宽带数据
FCC准备了关于美国宽带互联网接入状况的报告，数据对于测试的连接参数是有用的。我在这里列出关键图表，但完整的报告可在[这里](https://www.fcc.gov/reports-research/reports/measuring-broadband-america/measuring-fixed-broadband-report-2016#block-menu-block-4)。

## 一、连通性仿真（dummynet）
WebPagetest中连通性仿真的目的：是提供来自测试代理程序“最后一公里”的性能（但不要在进行物理分布的服务器上更改延迟）。这意味着，目标是从FCC报告中真实地模拟网络图，公共互联网段正常路由：

![](/assets/img/other/net.png)

## 二、延迟和广告带宽
在FCC测试中使用的测试服务器分布在测量位置附近，并根据供应商网络中的测量点进行验证，因此延迟测量应该是在dummynet中的网络部分的精确测量。

![](/assets/img/other/latency.png)

## 三、下行带宽
![](/assets/img/other/down.png)

## 四、上行带宽
![](/assets/img/other/up.png)