---
layout: post
title: Python发送Post请求
date: 2018-01-29 00:00:00 +0300
description: Python发送Post请求 # Add post description (optional)
img: workflow.jpg # Add image post (optional)
tags: [ssm] # add tag
---
爬虫:a program that automatically grabs Internet information from the Internet to capture valuable information about us.

# Python发送POST请求

> httpbin：测试 HTTP 请求及响应的网站.http://httpbin.org
>
> httpbin这个网站能测试 HTTP 请求和响应的各种信息，比如 cookie、ip、headers 和登录验证等，且支持 GET、POST 等多种方法，对 web 开发和测试很有帮助。

一个http请求包括三个部分，为别为请求行，请求报头，消息主体，类似以下这样：

> 请求行 
>
> 请求报头 
>
> 消息主体

HTTP协议规定post提交的数据必须放在消息主体中，但是协议并没有规定必须使用什么编码方式。服务端通过是根据请求头中的Content-Type字段来获知请求中的消息主体是用何种方式进行编码，再对消息主体进行解析，也就是说提交的数据格式要和Content-Type字段规定的一致。具体的编码方式包括：

- application/x-www-form-urlencoded 

  > 最常见post提交数据的方式，以form表单形式提交数据。

- application/json 

  > 以json串提交数据。

- multipart/form-data 

  > 一般使用来上传文件。

## 以form形式发送post请求

Reqeusts支持以form表单形式发送post请求，只需要将请求的参数构造成一个字典，然后传给requests.post( )的`data` 参数即可。

```python
import requests
import json
url = 'http://httpbin.org/post'
d = {'key1': 'value1', 'key2': 'value2'}
header = {"User-Agent":'SB'}  # 不指定返回就是"User-Agent":"python-requests/2.18.4"
r = requests.post(url, data=d, headers = header)
print(r.text)
```

输出：

```python
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "key1": "value1", 
    "key2": "value2"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Content-Length": "23", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "SB"  
  }, 
  "json": null, 
  "origin": "42.48.0.144", 
  "url": "http://httpbin.org/post"
}
```

可以看到，请求头中的Content-Type字段已设置为application/x-www-form-urlencoded.

且 `d = {'key1': 'value1', 'key2': 'value2'}` 以form表单的形式提交到服务端，服务端返回的form字段即是提交的数据。

## 以json形式发送post请求

可以将一json串传给requests.post()的data参数。

```python
import requests
import json
url = 'http://httpbin.org/post'
s = json.dumps({'key1': 'value1', 'key2': 'value2'}) 
r = requests.post(url, data=s)
print(r.text)
```

输出：

```python
{
  "args": {}, 
  "data": "{\"key1\": \"value1\", \"key2\": \"value2\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Content-Length": "36", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "json": {
    "key1": "value1", 
    "key2": "value2"
  }, 
  "origin": "42.48.0.144", 
  "url": "http://httpbin.org/post"
}
```

可以看到，请求头的Content-Type设置为application/json，并将`s`这个json串提交到服务端中。

另外也可以将一json串传给requests.post()的json参数。

```python
import requests
url = 'http://httpbin.org/post'
d = {'key1': 'value1', 'key2': 'value2'}
r = requests.post(url, json=d)
print(r.text)
```

结果：

```python
{
  "args": {}, 
  "data": "{\"key1\": \"value1\", \"key2\": \"value2\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Content-Length": "36", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "json": {
    "key1": "value1", 
    "key2": "value2"
  }, 
  "origin": "42.48.0.144", 
  "url": "http://httpbin.org/post"
}
```

- 注意：data参数和json参数在没有指定请求头的时候两者关键区别在于`Content-Type` 这个参数的有无。当然这个也是可以在请求头里面指定的

  ```python
  import requests
  import json
  url = 'http://httpbin.org/post'
  header = {"Content-Type": "application/json"}
  s = json.dumps({'key1': 'value1', 'key2': 'value2'}) 
  r = requests.post(url,data=s,headers=header)
  print(r.text)
  ```

## 以multipart形式发送post请求

Requests也支持以multipart形式发送post请求，只需将一文件传给requests.post()的`files`参数即可。

`ZHIHU/report.txt` 该文件下面就是Hello World!

```python
import requests
import json
url = 'http://httpbin.org/post'
files = {'file': open('ZHIHU/report.txt', 'rb')} # 以二进制格式打开一个文件用于只读
r = requests.post(url, files=files)
print(r.text)
```

输出：

```python
{
  "args": {}, 
  "data": "", 
  "files": {
    "file": "Hello World!"
  }, 
  "form": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Content-Length": "158", 
    "Content-Type": "multipart/form-data; boundary=3c0aa11dce0f481cb2f5061901adf31e", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "json": null, 
  "origin": "42.48.0.144", 
  "url": "http://httpbin.org/post"
}
```

文本文件report.txt的内容只有一行：Hello world!，从请求的响应结果可以看到数据已上传到服务端中。 boundary 用于分割不同的字段 *类似于查询字符串中的&*。不是HTML产生的，可以自己指定。