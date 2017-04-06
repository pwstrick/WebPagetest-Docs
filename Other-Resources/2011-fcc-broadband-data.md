# 2011 FCC Broadband Data
The FCC prepared a report on the state of Broadband Internet access in the US and the data is useful for deciding on connectivity parameters used for testing. 

## 一、Connectivity Emulation (dummynet)
The purpose of the connectivity emulation in WebPagetest is to provide realistic "last mile" performance from the test agents (but not to alter additional latency in getting to servers physically distributed).  This means that the goal is to realistically emulate parts 2-6 of the network diagram from the FCC report with the public Internet segment being routed as it would normally:

![](/assets/img/other/net.png)

## 二、Latency and Advertised Bandwidth
The test servers used in the FCC testing were geographically distributed close to the measurement locations and validated against measurement points within the provider's network so the latency measurements should be accurate measurements of the part of the network that we are looking to duplicate in dummynet.

![](/assets/img/other/latency.png)

## 三、Downstream Bandwidth
![](/assets/img/other/down.png)

## 四、Upstream Bandwidth
![](/assets/img/other/up.png)