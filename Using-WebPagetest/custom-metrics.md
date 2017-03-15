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

下面是一个收集3个不同指标的示例（2个数字和一个字符串）：
```javascript
[iframe-count]
return document.getElementsByTagName("iframe").length;

[script-tag-count]
return document.getElementsByTagName("script").length;

[meta-viewport]
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
如果使用API，自定义指标的表单字段为“custom” - 只需确保正确编码内容（如果使用GET，则为url encode，如果POST，则为form encode）。

## 四、静态指定自定义指标（私有实例）
每个指标在`settings/custom_metrics`下以`.js`扩展名作为单独文件存在。文件名将是度量的记录名称，执行代后返回一个值。  
例如，`settings/custom_metrics/meta-viewport.js`将定义一个自定义变量`meta-viewport` ，内容将如下所示：
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