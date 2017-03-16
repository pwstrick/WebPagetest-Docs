# 批处理库

## 一、API介绍
我们目前为用户提供以下Python API，帮助他们实现自己的特别批处理测试，添加更多的API。 如果你要添加新的API，可以通过详细说明向wpt-eng@google.com发送请求。我们将评估你的请求，并回复您。

## 二、API描述
### 2.1 创建批处理测试
```python
def ImportUrls(url_filename):
  """Load the URLS in the file into memory.

  Args:
    url_filename: the file name of the list of interested URLs

  Returns:
    The list of URLS
  """
```

### 2.2 提交批处理测试
```python
def SubmitBatch(url_list, test_params, server_url):
  """Submit the tests to WebPageTest server.

  Args:
    url_list: the list of interested URLs
    test_params: the user-configured test parameters
    server_url: the URL of the WebPageTest server

  Returns:
    A dictionary which maps a WPT test id to its URL if submission
    is successful.
  """
```

### 2.3 检查批处理测试的测试状态
```python
def CheckBatchStatus(test_ids, server_url):
  """Check the status of tests.

  Args:
    test_ids: the list of interested test ids
    server_url: the URL of the WebPageTest server
 
  Returns:
    A dictionary where key is the test id and content is its status.
  """
```

### 2.4 获取批处理测试的测试结果
```python
def GetXMLResult(test_ids, server_url):
  """Obtain the test result in XML format.

  Args:
    test_ids: the list of interested test ids
    server_url: the URL of WebPageTest server

  Returns:
    A dictionary where the key is test id and the value is a DOM object of the
    test result.
  """
```