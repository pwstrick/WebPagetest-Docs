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
#### 2.1.1 navigate
将浏览器导航到所提供的URL，并等待其完成。  
浏览器支持：IE，Chrome，Firefox，Safari
```bash
usage: navigate	<url>
example: navigate	http://webmail.aol.com

<url> - URL to provide the browser for navigation (same as you would enter into the address bar)
```

#### 2.1.2 click
触发标识的DOM元素的点击事件。此版本没有暗示的等待，脚本将在事件提交后继续运行（请参阅clickAndWait等待版本）。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: click	<attribute=value>
example: click	title=Delete (del)

<attribute'value> - DOM element to click on
```

#### 2.1.3 clickAndWait
触发标识的DOM元素的点击事件，然后等待浏览器活动完成。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: clickAndWait	<attribute=value>
example: clickAndWait	innerText=Send

<attribute'value> - DOM element to click on
```

#### 2.1.4 selectValue
从给定DOM元素的下拉列表中选择一个值。  
浏览器支持：IE
```bash
usage: selectValue	<attribute=value>	<value>
example: selectValue	id=country	usa

<attribute=value> - DOM element to select the value of
<value> - value to use
```

#### 2.1.5 sendClick / sendClickAndWait
创建JavaScript OnClick事件并将其发送到指定的元素。  
浏览器支持：IE
```bash
usage: sendClickAndWait	<attribute=value>
example: sendClickAndWait	innerText=Send

<attribute=value> - DOM element to send the click event to
```

#### 2.1.6 sendKeyDown / sendKeyUp / sendKeyPress (AndWait)
创建一个创建JavaScript键盘事件（OnKeyDown，OnKeyUp，OnKeyPress），并将其发送到指定的元素。  
浏览器支持：IE
```bash
usage: sendKeyDownAndWait	<attribute=value>    <key>
example: sendKeyDownAndWait	name=user    x

<attribute=value> - DOM element to send the click event to
<key> - Key command to send (special values are ENTER, DEL, DELETE, BACKSPACE, TAB, ESCAPE, PAGEUP, PAGEDOWN)
```

#### 2.1.7 setInnerHTML
将给定DOM元素的innerHTML设置为所提供的值。这通常用于填充类似可编辑的HTML面板（如webmail中的消息正文）。 如果要包括HTML格式，请使用此选项。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: setInnerHTML	<attribute=value>	<value>
example: setInnerHTML	contentEditable=true	%MSG%

<attribute=value> - DOM element to set the innerText of
<value> - value to use
```

#### 2.1.8 setInnerText
将给定DOM元素的innerText设置为所提供的值。这通常用于填充类似可编辑的HTML面板（如webmail中的消息正文）。 如果您不想包括任何HTML格式，请使用此选项。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: setInnerText	<attribute=value>	<value>
example: setInnerText	contentEditable=true	%MSG%

<attribute=value> - DOM element to set the innerText of
<value> - value to use
```

#### 2.1.9 setValue
将给定DOM元素的值属性设置为所提供的值。这通常用于填充页面上的文本元素（表单或其他形式）。当前只支持“input”和“textArea”元素类型。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: setValue	<attribute=value>	<value>
example: setValue	name=loginId	userName

<attribute=value> - DOM element to set the value of
<value> - value to use
```

#### 2.1.10 submitForm
触发标识窗体的提交事件。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: submitForm	<attribute=value>
example: submitForm	name=AOLLoginForm

<attribute=value> - Form element to submit
```

#### 2.1.11 exec
执行JavaScript。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: exec	<javascript code>
example: exec	window.setInterval('window.scrollBy(0,600)', 1000);
```

#### 2.1.12 execAndWait
执行JavaScript并等待浏览器完成从操作生成的任何活动。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: execAndWait	<javascript code>
example: execAndWait	window.setInterval('window.scrollBy(0,600)', 1000);
```

#### 2.1.13 fileDialog
单击标识的按钮以操作本地文件，指定所提供的文件，然后关闭本地文件浏览对话框。 这主要用于上传本地文件（附加到邮件，上传图片等）。  
浏览器支持：IE
```bash
usage: fileDialog	<attribute=value>	<file>
example: fileDialog	type=file	msg.gif

<attribute=value> - DOM element to click on.
<file> - file to attach/upload.
```

除非一个页面具有多个用于附加文件的按钮（极不可能），否则应该能够始终对DOM元素使用`type=file`。  
文件的路径搜索顺序是首先检查脚本运行所在的同一目录，然后检查WebPagetest安装到的目录，然后最终将该文件作为绝对路径。

### 2.2 结束条件（End Conditions）
#### 2.2.1 requiredRequest
强制测试在完成之前等待给定的请求。  
浏览器支持：IE
```bash
usage: requiredRequest	<url substring>
example: requiredRequest	adsWrapper.js

<url substring> - Part of the request URL to match (case-sensitive substring match)
```
你可以通过为每个新请求重复此命令来指定多个请求：

```bash
requiredRequest	www.aol.com
requiredRequest	www.google.com
navigate	www.google.com
```

#### 2.2.2 setABM
设置“基于活动的测量（Activity Based Measurement）”模式。 有效值为：  
+ 0 - 禁用（Web 1.0 - 基于文档完成的测量）
+ 1 - 启用（Web 2.0 - 测量直到活动停止）
+ 2 - 自动（测量直到活动停止，但记录是否应使用文档完成或完全加载数字）

如果在脚本中未指定，则默认值为2（自动）  
浏览器支持：IE
```bash
usage: setABM	<mode>
example: setABM	0

<mode> - ABM mode to use
```

#### 2.2.3 setActivityTimeout
覆盖在完成测试之前的，最后一次网络活动之后的，超时值（默认为2000秒，即2秒）。  
浏览器支持：IE, Chrome, Firefox, Safari
```bash
usage: setActivityTimeout	<timeout in milliseconds>
example: setActivityTimeout	5000

<timeout in milliseconds> - Number of milliseconds after the last network activity (after onload) before calling a test done.
```

#### 2.2.4 setDOMElement
设置下一个事件成功完成所需的DOM元素的属性。直到DOM元素出现或动作超时，测量才完成。此外，DOM元素出现的时间将记录在结果中。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: setDOMElement	<attribute=value>
example: setDOMElement	name=loginId

<attribute=value> - DOM element to wait for
```

#### 2.2.5 setDOMRequest
设置下一个事件成功完成所需的Http请求的子字符串。在特定Http请求完成或操作超时之前，测量将不会完成。  
此外，Http请求发生的时间将记录在结果中。  
浏览器支持：IE
```bash
usage: setDOMRequest	<target>	<value>
example: setDOMRequest	www.google.com/images

<target> - Http Request to wait for
<value> - It is optional. It can be TTFB or START. If it is not present, the time till the END of the Http Request will be recorded in the results 
```

#### 2.2.6 setTimeout
覆盖单个脚本步骤的超时值。  
浏览器支持：IE, Chrome, Firefox, Safari
```bash
usage: setTimeout	<timeout in seconds>
example: setTimeout	240

<timeout in seconds> - Number of seconds to allow for the navigation/step to complete.
```

#### 2.2.7 waitForComplete
等待当前活动的测量完成。这通常不需要，因为启动活动的大多数命令隐式等待或具有包括等待的命令的版本。这应该只在没有一个适合于被测量的动作时使用。  
浏览器支持：IE
```bash
usage: waitforComplete
example: waitforComplete
```

#### 2.2.8 waitForJSDone
等待页面上的代码通过调用`window.webpagetest.done（）`来显式地指示它已完成。  
浏览器支持：IE
```bash
usage: waitForJSDone
example: waitForJSDone
```
页面上的JavaScript看起来像：

```JavaScript
if( window.webpagetest )
    window.webpagetest.done();
```

### 2.3 请求操作（Request Manipulation）
#### 2.3.1 block
阻止个别请求加载（可用于阻止像广告等内容）。该命令匹配要阻止的事物列表与每个请求的完整URL（包括主机名）。  
浏览器支持：IE, Chrome, Firefox
```bash
usage: block    <block strings>
example: block    adswrapper.js addthis.com

<block strings> - space-delimited list of substrings to block
```

#### 2.3.2 blockDomains
阻止来自指定网域的所有请求加载（可用于阻止像广告等内容）。使用以空格分隔的完整域的列表进行阻止。  
浏览器支持：桌面（wptdriver 300+）
```bash
usage: blockDomains    <block domains>
example: blockDomains    adswrapper.js addthis.com

<block domains> - space-delimited list of domains to block
```

#### 2.3.3 blockDomainsExecpt
阻止不是来自某个指定网域的所有请求加载（可用于阻止像广告等内容）。以允许的完整域的空格分隔列表。  
浏览器支持：桌面（wptdriver 300+）
```bash
usage: blockDomainsExcept    <allow domains>
example: blockDomainsExcept    www.example.com cdn.example.com

<allow domains> - space-delimited list of domains to allow
```

#### 2.3.4 setCookie
存储浏览时要使用的浏览器Cookie。  
浏览器支持：IE，Chrome，Firefox
```bash
usage: setCookie	<path>	<value>
example: setCookie	http://www.aol.com	zip=20166
example: setCookie	http://www.aol.com	TestData = Test; expires = Sat,01-Jan-2000 00:00:00 GMT

<path> - Path where the cookie should be used/stored
<value> - Cookie value (if expiration information isn't included it will be stored as a session cookie)
```

#### 2.3.5 setDns
允许覆盖用于主机名的IP地址。仍将查找原始名称，以便记录准确的时间，但地址将被指定的替换。  
浏览器支持：IE, Chrome, Firefox, Safari
```bash
usage: setDns	<host name>	<IP Address>
example: setDns	www.aol.com	127.0.0.1

<host name> - Host name for the DNS entry to override
<IP Address> - IP Address for the host name to resolve to
```

#### 2.3.6 setDNSName
允许覆盖主机名（创建假CNAME）。  
浏览器支持：IE, Chrome, Firefox, Safari
```bash
usage: setDnsName	<name to override>	<real name>
example: setDnsName	pat.aol.com	www.aol.com

<name to override> - Host name to replace
<real name> - Real name to lookup instead
```

#### 2.3.7 setUserAgent
覆盖由浏览器发送的用户代理字符串。  
浏览器支持：IE, Chrome, Firefox, Safari  
小心：你仍然使用相同的浏览器引擎，因此仍然受到该浏览器的功能和行为的限制，即使欺骗了另一个浏览器。
```bash
usage: setUserAgent    <user agent string>
example: setUserAgent    Mozilla/5.0 (iPhone; U; CPU like Mac OS X; en) AppleWebKit/420+ (KHTML, like Gecko) Version/3.0 Mobile/1A543 Safari/419.3

<user agent string> - User agent string to use.
```

#### 2.3.8 overrideHost
使用提供的值，替换给定主机的Host：HTTP头的值。它还添加了一个带有原始值的新标题（x-Host :)。  
浏览器支持：IE，Chrome，Firefox，Safari（无SSL）
```bash
usage: overrideHost	<host>    <new host>
example: overrideHost	www.aol.com    www.notaol.com

<host> - host for which you want to override the Host: HTTP header
<new host> - value to set for the Host header
```

#### 2.3.9 overrideHostUrl
对于给定主机的所有请求，重写请求以转到其他服务器，并将原始主机包含在新URI中。  
浏览器支持：IE
```bash
usage: overrideHostUrl	<host>    <new host>
example: overrideHostUrl	www.webpagetest.org    staging.webpagetest.org

<host> - host for which you want to redirect requests
<new host> - target server to receive the redirected requests

In this example, http://www.webpagetest.org/index.php will get rewritten to actually request http://staging.webpagetest.org/www.webpagetest.org/index.php
```

#### 2.3.10 addHeader
将指定的标头添加到每个http请求（除了存在的标头之外，不覆盖现有标头）。  
浏览器支持：IE，Chrome，Firefox，Safari（无SSL）
```bash
usage: addHeader	<header>    {filter}
example: addHeader	Pragma: akamai-x-cache-on

<header> - Full header entry to add (including label)
{filter} - (optional) regex match for host names where the header should be added
```

#### 2.3.11 setHeader
将指定的标头添加到每个http请求，覆盖标头（如果标头已存在）。  
浏览器支持：IE，Chrome，Firefox，Safari（无SSL）
```bash
usage: setHeader	<header>    {filter}
example: setHeader	UA-CPU: none-ya

<header> - Full header entry to set (including label)
{filter} - (optional) regex match for host names where the header should be set
```

#### 2.3.12 resetHeaders
清除通过addHeader或setHeader指定的任何标头（以防只想覆盖部分脚本的标头）。  
浏览器支持：IE，Chrome，Firefox，Safari
```bash
usage: resetHeaders
example: resetHeaders
```

### 2.4 杂项（Misc）
#### 2.4.1 combineSteps
使多个脚本步骤在结果中合并为单个“步骤”  
浏览器支持：IE，Chrome，Firefox，Safari
```bash
usage: combineSteps	[count]
example: combineSteps

[count] - Number of script steps to merge (optional, defaults to 0 which is ALL)
```

示例脚本：
```bash
combineSteps
navigate	www.google.com
navigate	www.yahoo.com
navigate	www.aol.com
```

#### 2.4.2 if/else/endif
根据测试的运行编号或缓存状态有条件地执行一个脚本代码块。  
条件块可以嵌套。  
浏览器支持：IE，Chrome，Firefox，Safari
```bash
usage:  if	[cached|run]    <value>
        else
        endif

example:    if    run    1
            if    cached    0
            <do something for first view of first run>
            endif
            else
            <do something else for everything but first run>
            endif

[cached|run] - Compare against run number or cached state
<value> - matching run number or cached state to execute block
```

#### 2.4.3 ignoreErrors
继续运行脚本，而不考虑任何错误。如果你希望脚本的某些部分间歇性失败，但仍希望继续执行脚本，这将非常有用。默认值是禁用它，处理脚本中的任何错误都将停止测试。  
浏览器支持：IE
```bash
usage: ignoreErrors	<ignore>
example: ignoreErrors	1

<ignore> - set to 1 to turn on error ignoring and 0 to disable it
```

#### 2.4.4 logErrors
启用或禁用错误日志。默认值为启用。
浏览器支持：IE
```bash
usage: logErrors	<log>
example: logErrors	0

<log> - set to 0 to turn off error logging and 1 to re-enable it
```

#### 2.4.5 loadFile
用提供的文件的内容填充提供的变量（内容预期为文本，而不是二进制）。  
脚本引擎将自动替换值参数字段（第3个参数）中任何变量的所有出现。对变量如何命名没有限制，但强烈建议您使用％variable％或$ variable $或类似的约定，以便不会意外替换您不希望发生的变量。  
对可以使用的变量数量或可以加载的文件数量没有限制。每个新文件只是添加到现有变量列表。如果两个文件之间的变量名存在冲突，则最后装入的文件将替换所有先前的版本。
浏览器支持：IE
```bash
usage: loadFile	<file>	<variable>
example: loadFile	msg.txt	%MSG%

<file> - file contents that are to be loaded into the provided variable name.
<variable> - variable name to associate with the file contents.
```
文件的路径搜索顺序是首先检查脚本运行所在的同一目录，然后检查WebPagetest安装到的目录，然后最终将该文件作为绝对路径。

#### 2.4.6 loadVariables
从提供的文件填充一组变量。 该文件应该是一个平面文本文件，每行上的变量格式为“`variable = value`”。  
浏览器支持：IE
```bash
%AOLSN%=username
%AOLPW%=password
{noformat}

The script engine will automatically replace all occurrences of any variable in the value parameter field (3rd parameter).  There is no restriction on how the variables are named but it is strongly recommended that you use a convention of %variable% or $variable$ or something similar so that you don't accidentally substitute variables where you did not expect it to happen.

There is no limit on the number of variables you can use or the number of variable files you can load.  Each new variable file just adds to the existing variable list.  If there is a collision in variable names between two files, the one that is loaded last replaces all previous versions.

<code><pre>
usage: loadVariables	<variable file>
example: loadVariables	accounts.txt

<variable file> - file that includes the variable/value pairs (see format above).
```

#### 2.4.7 minInterval
指定脚本的以下部分可以运行的最小时间间隔（以分钟为单位）。例如，如果只想测试邮件发送的频率不要超过每60分钟（在单个机器上），那么将在脚本中要发送邮件的位置插入一个minInterval命令。如果间隔尚未过期，则脚本将退出。还可以使用它来确保多个浏览器不会同时执行同一操作，通过指定间隔0（这将允许运行测试，但会暂停第二个脚本，直到第一个脚本完成为止）。  
浏览器支持：IE
```bash
usage: minInterval	<key>	<interval>
example: minInterval	AOLSendMail	60

<key> - Unique event name that will be restricted to the interval
<interval> - Minimum time in minutes to allow between allowing the event to run
```

#### 2.4.8 endInterval
结束由minInterval块保护的代码块。如果要使用间隔保护的脚本部分位于脚本中间，并且总是想要执行脚本的其余部分（例如注销），这将非常有用。  
浏览器支持：IE
```bash
usage: endInterval
example: endInterval
```