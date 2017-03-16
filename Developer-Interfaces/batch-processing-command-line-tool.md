# 批处理命令行工具

## 一、工具介绍
此命令行工具执行简单的一次性批处理测试。它将批量加载输入文件中的一组URL，将所有URL提交到执行测试的WebPageTest服务器，然后下载成功测试的结果并报告失败的测试。该工具主要由我们的批处理库中的API实现，因此也可以作为批处理API的示例使用。

## 二、用法说明
```python
Usage: wpt_batch.py [options]

Options:

  -h, --help    
  显示此帮助消息并退出
  
  -s SERVER, --server=SERVER    
  wpt服务器URL。 默认值为“http://www.webpagetest.org/”，它是公用实例，但需要API密钥。
  
  -i URLFILE, --urlfile=URLFILE     
  输入网址文件的路径（文件的每一行都应为http网址，例如“http://www.google.com/”。
  
  -f OUTPUTDIR, --outputdir=OUTPUTDIR    
  要保存测试结果的输出目录的路径。默认值是当前目录下名为“result”的子目录。测试结果文件由url_wpt-test-id.xml命名。
  
  -y CONNECTIVITY, --connectivity=CONNECTIVITY
  将连接设置为预定义类型：DSL，拨号（Dial），Fios和自定义（区分大小写）。当是自定义连接时，您可以使用以下选项-u / d / l / p设置自定义连接。

  -u BWUP, --bwup=BWUP  
  测试的上传带宽（单位：kbps）。默认值为1500（即1.5Mbps）。
  
  -d BWDOWN, --bwdown=BWDOWN    
  下载带宽（单位：kbps）的测试。 默认值为384。

  -l LATENCY, --latency=LATENCY    
  RTT（单位：ms）。

  -p PLR, --plr=PLR     
  测试的包丢失（百分比）率。默认值为0。

  -v FVONLY, --fvonly=FVONLY    
  仅第一视图。重复视图通常用于测试缓存。默认值为True。

  -t, --tcpdump         
  启用tcpdump。默认值为False。

  -c SCRIPT, --script=SCRIPT    
  托管脚本文件

  -r RUNS, --runs=RUNS  
  每次测试的运行次数。默认值为9。

  -o LOCATION, --location=LOCATION    
  测试位置。默认位置是Dulles。

  -m MV, --mv=MV        
  仅为中值运行（median run）保存视频。默认值为1。
```

## 三、用法示例
### 3.1 所有默认设置的批处理测试
```python
./wpt_batch.py
```
此测试使用所有默认配置。该脚本读取`./urls.txt`文件，将所有文件提交到`http：// latencylab WPT服务器`并将结果（以XML格式）保存在目录`./result`中。用DSL连接，重复测试9次，并不丢包。
### 3.2 使用用户指定的输入文件，连接条件和运行次数进行批处理测试
```python
./wpt_batch.py --urlfile=/foo/bar/urls.txt --runs=3 --connectivity=custom --bwup=384 --bwdown=1500 --latency=100 --plr=1
```
此测试从`/foo/bar/urls.txt`加载网址。 所有测试重复3次，`384kbps`上传带宽，`1500kbps`下载带宽，100ms往返时间，丢包率1％。
### 3.3 使用用户指定的脚本进行帐户登录的测试