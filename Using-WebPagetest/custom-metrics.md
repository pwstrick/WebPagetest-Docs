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