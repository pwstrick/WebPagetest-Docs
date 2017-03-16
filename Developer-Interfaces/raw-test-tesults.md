# 原始测试结果
## 一、页面数据字段
这些是页面级（摘要）结果文件中，从“Raw Page data”链接导出的CSV文件中列。 

    1. Date 
    2. Time 
    3. Event Name 
    4. URL 
    5. Load Time (ms)
    6. Time to First Byte (ms)
    7. unused
    8. Bytes Out - 这将总是包含测试的总和，而不考虑测量类型 - Added in build 53
    9. Bytes In - As of build 53 this will always contain the total for the test, regardless of measurement type
    10. DNS Lookups - 同上
    11. Connections - 同上
    12. Requests - 同上
    13. OK Responses - 同上
    14. Redirects - 同上
    15. Not Modified - 同上
    16. Not Found - 同上
    17. Other Responses - 同上
    18. Error Code
    19. Time to Start Render (ms)
    20. Segments Transmitted
    21. Segments Retransmitted
    22. Packet Loss (out)
    23. Activity Time(ms)
    24. Descriptor
    25. Lab ID
    26. Dialer ID
    27. Connection Type
    28. Cached
    29. Event URL
    30. Pagetest Build
    31. Measurement Type - （DWORD - 1表示Web 1.0，2表示Web 2.0）
    32. Experimental (DWORD)
    33. Document Complete Tims (ms)
    34. Event GUID - （与对象数据中的事件GUID匹配） - Added in build 42
    35. DOM Element Time (ms) - 在build 45中添加
    36. Includes Object Data - 指示对象数据是否可用的标志（1表示yes，0表示否）
    37. Cache Score - 具有过期标头的静态资产百分比，以便启用浏览器缓存 - Added in build 51
    38. Static CDN Score - 从CDN提供的静态资产的百分比 - Added in build 51
    39. One CDN Score - 100 - 10 *不同的CDN的使用数量 - Added in build 51
    40. GZIP Score - gzip编码的文本或js资源的百分比 - Added in build 51
    41. Cookie Score - 写入Cookie而不是aol.com域的响应的百分比 - Added in build 51
    42. Keep-Alive Score -服务器中使用持久连接的响应百分比 - Added in build 51
    43. DOCTYPE Score - 具有有效DOCTYPE的html或xhtml资源的百分比
    44. Minify Score - 被缩小的html或js资源的百分比（小于10％的行有前导/尾随空格或注释），在build 51中添加
    45. Combine Score - 从给定主机提供的该类型的唯一资产的js或css资产的百分比（来自同一主机的多个类型中的多个可以组合），在build 51中添加
    46. Bytes Out (Document) - Added in build 53
    47. Bytes In (Document) - Added in build 53
    48. DNS Lookups (Document) - Added in build 53
    49. Connections (Document) - Added in build 53
    50. Requests (Document) - Added in build 53
    51. OK Responses (Document) - Added in build 53
    52. Redirects (Document) - Added in build 53
    53. Not Modified (Document) - Added in build 53
    54. Not Found (Document) - Added in build 53
    55. Other Responses (Document) - Added in build 53
    56. Compression Score - 通过图像压缩测试的图像对象百分比 - Added in build 54
    57. Host - 检索的URL的主机名部分 - Added in build 57
    58. IP Address - 页面上第一个请求的IP地址 - Added in build 66
    59. ETag Score - 通过ETag标头检查的请求百分比- Added in build 170
    60. Flagged Requests - 计数“标记”请求，内部仅在此时使用 - Added in build 179
    61. Flagged Connections - 计数“标记”网络的连接 - Added in build 179
    62. Max Simultaneous Flagged Connections - 同时“标记”连接的最大数量的计数 - Added in build 179
    63. Time to Base Page Complete (ms) - 从测试开始到基本HTML页面完成下载的时间（包括DNS查找，套接字连接，重定向等） - Added in build 179
    64. Base Page Result - 基页（200,404等）的响应代码。如果重定向到位，这将是重定向后基本网页的结果 - Added in build 179
    65. Gzip Total Bytes - Gzip压缩的适用资源总大小 - Added in build 179
    66. Gzip Savings - 可以通过Gzip压缩保存的字节 - Added in build 179
    67. Minify Total Bytes - 缩小的适用资源总大小 - Added in build 179
    68. Minify Savings - 通过缩小保存的字节数 - Added in build 179
    69. Image Compression Total Bytes - 图像压缩的适用资源总大小 - Added in build 179
    70. Image Compression Savings - 通过图像优化保存的字节 - Added in build 179
    71. Base page redirect count - 基页的重定向数 - Added in build 180
    72. Optimization Checked - 如果运行优化检查，则为1，如果不是，则为0 - Added in build 209
    73. Above-the-Fold Time (ms) - 时间直到above-the-fold稳定（如果明确要求） - Added in build 260
    74. DOM Element Count - 文档上的DOM元素数（包括框架中的子文档） - Added in build 274
    75. Page Speed Version - 使用的Page Speed SDK版本 - Added in build 281
    76. Page Title - 所得文件的标题（最终标题） - Added in build 289
    77. Time to Title - 从操作开始到标题第一次更改的时间（以ms为单位） - Added in build 289
    78. Load Event Start
    79. Load Event End
    80. DOM Content Ready Start
    81. DOM Content Ready End
    82. Last Visual Change - 最后一次可视更改页面的时间（以ms为单位，仅在启用视频捕获时可用）
    83. Browser Name
    84. Browser Version
    85. Base Page Server Count - 为基本网页的DNS查找返回的IP地址数
    86. Server RTT - 到基本服务器的估计往返时间（从基页的套接字连接时间获取），如果不可用，则为空
    87. Base Page CDN - 用于提供基本HTML网页的CDN的名称（如果有）
    88. Adult Site Flag - 0不可能是成人网站，1可能是成人内容
    89. Fixed Viewport - 1的页面定义了一个元视口
    90. Progressive JPEG Score - 1为N / A，否则为渐进式JPEG的JPEG字节的百分比
    91. First Paint - 浏览器报告的第一个绘制时间（IE特定属性 - window.performance.timing.msFirstPaint）
    92. Peak Memory across browser processes (deprecated)
    93. Peak browser process count (deprecated)
    94. Doc Complete CPU Time (ms)
    95. Fully Loaded CPU Time (ms)
    96. Doc Complete CPU Utilization (%)
    97. Fully Loaded CPU Utilization (%)
    98. Is Site Responsive (deprecated)
    99. Browser Process Count - 在页面测试结束时测量
    100. Working set size for the main browser process (KB) - 在页面测试结束时测量
    101. Child Private Working Set Size (KB) - 除了主进程之外的所有浏览器进程的私有工作集合总和，在页面测试结束时测量
    102. Time to DOM Interactive - 从导航（Navigation）时间
    103. Time to DOM Loading - 从导航时间
    104. Base Page TTFB - 从请求到基本页面元素的响应的时间
    105. Visually Complete - 第一次当视觉进度达到100％
    106. SpeedIndex - 计算速度指数
    107. Certificate Bytes - 服务器提供的TLS证书中的总字节数
## 二、请求数据字段


## 三、聚合结果（批量测试）