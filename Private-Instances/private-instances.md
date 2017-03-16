# 私有实例
WebPagetest可安装和运行私有实例的软件包。

## 一、发布
最新版本是[3.0](https://github.com/WPO-Foundation/webpagetest/releases/tag/WebPageTest-3.0)

## 二、轻松部署（在EC2上）
有一个[EC2 AMI](https://github.com/WPO-Foundation/webpagetest/blob/master/docs/EC2/Server%20AMI.md)用于WebPageTest服务器，可以有很多好处：
+ 无需配置（在5分钟内启动并运行）。
+ 根据需要在所有EC2（亚马逊弹性计算云）区域中自动启动和停止EC2测试代理。
+ 自动更新到最新的服务器和代理代码。

## 三、手动部署
### 3.1 系统要求
WebPageTest可以配置在一个系统上运行（Web服务器和测试机器都在同一台PC上运行），或者为Web服务器和测试人员使用单独的系统。

### 3.2 Web服务器
Web服务器可以是任何支持PHP的操作系统（Linux或Windows）。
+ Nginx (推荐php-fpm) 或者 Apache 2.x+:
+ PHP 5.3.0 或更新版本，包括以下模块:
    + gd
    + zip
    + zlib
    + mbstring
    + curl (如果你想使用远程存储，像S3和谷歌存储)
    + sqlite (如果你想要能够编辑测试标签)
+ ffmpeg 
    + 在路径中可用（用于提取视频缩略图）
    + 对于移动代理视频支持，需要1.x（Linux，Windows）
+ imagemagick (可选)
    + 如果使用移动Node.js代理，则需要移动视频处理
+ jpegtran (可选)
    + 在路径中可用（用于jpeg分析）
+ exiftool (可选)
    + 在路径中可用（用于jpeg分析）

### 3.3 测试机
Windows（Vista或更高版本）（如果使用64位，需要WebPagetest 2.9或更高版本支持流量整形）

## 四、Configuring
`zip`存档包含2个文件夹。`www`文件夹是Web服务器软件，代理文件夹用于测试机器。存档中的配置文件具有`.sample`扩展名，因此如果要更新现有安装，则可以使用存档中的新文件覆盖当前文件。  
`2.8`的新版本是一个脚本，将检查常见的配置问题。 可以通过`http://<your web server>/install`访问。

### 4.1 Web Server
1. 使用所需的模块配置Apache，并设置为允许覆盖.htaccess。配置文件可能如下所示：
```bash
<Directory "/var/www/webpagetest">
        AllowOverride all
        Order allow,deny
        Allow from all
</Directory>
<VirtualHost *:80>
        DocumentRoot /var/www/webpagetest
</VirtualHost>
```
2. 如果将使用代理，需要上传`.pcap`文件。在`php.ini`中将`upload_max_filesize`和`post_max_size`设置为大值（`10mb`应该足够）。  
3. 如果要收集Chrome开发工具跟踪，请考虑将`memory_limit`设置为较大值或通过将其设置为-1来禁用内存限制。    
4. 使用`PHP DSO`处理程序`mod_php`可以大幅减少CPU使用率。  
5. 重新启动Apache以使用新的配置设置。  
6. 将文件从归档中的`www`文件夹复制到DocumentRoot位置（例如`/var/www/webpagetest`）。  
7. 授予Apache用户对DocumentRoot下的以下文件夹的读/写权限：
    + tmp
    + dat
    + results
    + work/jobs
    + work/video
    + logs
8. 在设置文件夹中有几个设置文件可用于配置站点。制作所有`.sample`文件的副本（删除`.sample`扩展名）作为配置设置的模板。大多数设置可以按原样使用，除了`locations.ini`（特别是如果要配置多个测试位置）。  
9. 有关配置`locations.ini`的详情，请访问：[locations](https://sites.google.com/a/webpagetest.org/docs/private-instances/locations)

### 4.2 Test Machine(s)
#### 4.2.1 Headless Servers (including Google Compute Engine)
### 4.3 Mobile
## 五、EC2 Test Agents
### 5.1 Configuration
#### 5.1.1 Parameters
#### 5.1.2 Example User Data string
#### 5.1.3 Sample locations.ini
### 5.2 AMI Images
#### 5.2.1 Finding the Images (aka EC2 AMI search sucks)
#### 5.2.2 us-east (Virginia)
#### 5.2.3 us-east-2 (Ohio)
#### 5.2.4 us-west (California)
#### 5.2.5 us-west-2 (Oregon)
#### 5.2.6 ca-central-1 (Canada Central)
#### 5.2.7 eu-west-1 (Ireland)
#### 5.2.8 eu-west-2 (London)
#### 5.2.9 eu-central (Frankfurt)
#### 5.2.10 ap-northeast-1 (Tokyo)
#### 5.2.11 ap-northeast-2 (Seoul)
#### 5.2.12 ap-southeast-1 (Singapore)
#### 5.2.13 ap-southeast-2 (Sydney)
#### 5.2.14 ap-south-1 (Mumbai)
#### 5.2.15 sa-east (Sao Paulo)
## 六、Updating Test Agents
## 七、Troubleshooting
### 7.1 Web Server
#### 7.1.1 Test Agent