# 自定义指标
WebPagetest可以在测试结束时执行任意的JavaScript来收集自定义指标。这些可以在服务器配置中静态地定义，或者在运行时每个测试中指定。  
JavaScript应该写得像是在执行一个函数调用，最后返回自定义指标。下面是一个查找页面的元视口并提取它的示例：
```javascript
var viewport = undefined;
var metaTags=document.getElementsByTagName("meta");
for (var i = 0; i < metaTags.length; i++) {
    if (metaTags[i].getAttribute("name") == "viewport") {
        viewport = metaTags[i].getAttribute("content");
        break;
    }
}
return viewport;
```

## 一、支持的浏览器
+ Internet Explorer
+ Chrome
+ Firefox

## 二、注意事项
+ 脚本必须阻塞，并直接返回感兴趣的数据。不支持异步操作（计时器，RequestAnimationFrame，Ajax请求等）。
+ 自定义指标与内置指标存在于同一命名空间中，如果它们具有相同的名称，则可以覆盖内置指标。
+ 指标名称应该是简单的字母数字，并且可能没有空格。

## 三、在测试时指定自定义指标
为了简单起见，度量的文本输入框以ini文件的形式定义：
```bash
[metric-name]
<code>

[metric-2-name]
<metric 2 code>
```