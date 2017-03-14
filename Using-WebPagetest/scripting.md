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

对于表单字段，通常最好使用名称属性，这些将被上传到服务器。类属性是特殊的，并且被引用为`className`而不是类。除了DOM元素属性匹配之外，还有两个特殊属性可用于匹配内容。 innerText和innerHtml，它们都将匹配DOM元素的内容，而不是它的属性。例如：
```html
<div dojoattachpoint="containerNode" class="label">Delete</div>
```
可以通过`innerText = Delete`来标识。 匹配区分大小写，并匹配完整字符串。
