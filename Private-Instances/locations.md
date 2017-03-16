# 地点
配置WebPagetest实例的一个更困难的方面是设置位置的配置并将其与测试代理上的位置匹配。

在本例中，我们将配置4个不同的测试机器：
+ 机器＃1 - 配置有安装在内部办公网络上的IE 8，Chrome和Firefox的PC
+ 机器＃2 - 配置有位于Virginia的IE 8和Chrome的PC
+ 机器＃3 - 在California配置有IE 8的PC
+ 机器＃4 - 在California配置有IE 9的PC

逻辑上配置将如下所示：

![](/assets/img/private/locations.png)

有3个不同的物理位置 - Office，Virginia和California。 这些将显示在用户界面中的“测试位置”下拉列表中。  
Office和Virginia计算机将运行URLBlast（对于IE）和WptDriver（对于Chrome和Firefox）。 每个加利福尼亚州的计算机将只运行URLBlast（对于IE）。 每个软件实例（URLBlast或WptDriver）都指向服务器上特定于该机器配置（在locations.ini配置中的叶节点）的配置。  
这里是上面例子中的locations.ini配置：
```bash
[locations]
1=Office
2=Virginia
3=California
default=Office

; These are the top-level locations that are listed in the location dropdown
; Each one points to one or more browser configurations

[Office]
1=Office_IE8
2=Office_wptdriver
label="Office LAN (Virginia - IE8,Chrome,Firefox)"

[Virginia]
1=Virginia_IE8
2=Virginia_wptdriver
label="Virginia (IE8,Chrome)"

[California]
1=California_IE8
2=California_IE9
label="California (IE8,9)"

; These are the browser-specific configurations that match the configurations
; defined in the top-level locations.  Each one of these MUST match the location
; name configured on the test agent (urlblast.ini or wptdriver.ini)

[Office_IE8]
browser=IE 8
label="Office LAN - IE8"

[Office_wptdriver]
browser=Chrome,Firefox
label="Office LAN"

[Virginia_IE8]
browser=IE 8
label="Virginia - IE8"

[Virginia_wptdriver]
browser=Chrome
label="Virginia"

[California_IE8]
browser=IE 8
label="California - IE8"

[California_IE9]
browser=IE 9
label="California - IE9"
```

*重要信息：“Office”，“Virginia”和“California”是在UI中使用的逻辑分组，但测试计算机将永远不会被配置为指向它们。 UrlBlast.ini和wptdriver.ini必须指向浏览器特定的配置。*

如果映射设置不正确，还是可以提交测试，但它们永远不会被处理，因为测试机器没有使用正确的位置ID连接。