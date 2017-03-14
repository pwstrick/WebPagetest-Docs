# 脚本
&emsp;&emsp;WebPagetest具有脚本功能，可自动执行多步测试（例如，登录网站或发送电子邮件）。脚本包含在文件中，其中单个文件构成浏览器会话（浏览器将在脚本完成后关闭）。文件是纯文本文件，可以由任何文本编辑器编辑。

你可以通过从“文件”菜单中选择“Run Script”来测试桌面版本中的脚本。

&emsp;&emsp;脚本文件的每一行包含一个命令和任何必要的参数，并且以制表符分隔（即命令之后是一个制表符，然后是第一个参数，然后是一个制表符和第二个参数等，直到该行结束）。 参数的数量及其控制取决于命令。

以//开头的空行和行将被忽略，因此您可以在脚本中嵌入注释。

&emsp;&emsp;对DOM元素操作的脚本命令标识具有`attribute = value`格式的DOM元素，其中该属性标识要作用的DOM元素的唯一属性。例如，如果正在填写表单，填充的元素看起来像这样：
```html
<input type="text" class="tabInputFields" id="lgnId1" value="" tabindex="1" maxlength="97" name="loginId"/>
```
你可以将它标识为`id = lgnId1`，`name = loginId`或`tabindex = 1`

&emsp;&emsp;对于表单字段，通常最好使用名称属性，这些将被上传到服务器。类属性是特殊的，并且被引用为`className`而不是类。除了DOM元素属性匹配之外，还有两个特殊属性可用于匹配内容。 innerText和innerHtml，它们都将匹配DOM元素的内容，而不是它的属性。例如：
```html
<div dojoattachpoint="containerNode" class="label">Delete</div>
```
可以通过`innerText = Delete`来标识。 匹配区分大小写，并匹配完整字符串。

## 一、托管脚本（Hosted Scripting）
托管版本的WebPagetest支持执行上传的脚本，但有一些限制：
+ 托管的脚本只能有一个步骤产生数据（见下面的例子，如何抑制中间步骤的结果）
+ 不允许使用使用外部文件的命令（loadFile，loadVariables，fileDialog）

为了抑制中间步骤，您需要确保对于要记录的步骤，禁用数据记录。例如：
```bash
logData    0
// put any urls you want to navigate
navigate    www.aol.com
navigate    news.aol.com
logData    1
// this step will get recorded
navigate    news.aol.com/world
```
上面的脚本将导航到主要的aol门户，然后到新闻页面，最后到世界新闻特定页面（仅记录世界新闻页面的记录结果）。 这样就可以测试给定路径对网站的性能（例如，共享css和js缓存）的影响。

另一个重要的用例是，如果你想测试一个需要验证的网站。以下是验证脚本：
```bash
logData	0
// bring up the login screen
navigate	http://webmail.aol.com
logData	1
// log in
setValue	name=loginId	someuser@aol.com
setValue	name=password	somepassword
submitForm	name=AOLLoginForm
```
你不会得到很多关于脚本失败的反馈，所以请确保在桌面版本的WebPagetest（File->Run Script）中测试脚本，然后再上传它们进行托管测试。

## 二、命令参考（Command Reference）
### 2.1 导航/ DOM互动（Navigation/DOM Interaction）
#### 2.1.1 导航（navigate）
将浏览器导航到所提供的URL，并等待其完成。

浏览器支持：IE，Chrome，Firefox，Safari。
```bash
usage: navigate	<url>
example: navigate	http://webmail.aol.com

<url> - URL to provide the browser for navigation (same as you would enter into the address bar)
```
