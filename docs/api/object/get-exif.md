# GetExif

### 功能介绍
用于获取图片的详细信息，包括图像信息和拍摄信息。

###  请求语法
```
HEAD /<BucketName>/<ObjectName>?exif HTTP/1.1
Host: wos.<Endpoint>
Date: <date>
Authorization: <authorization>
```
**URI参数**
| 参数名称 | 类型 | 描述 | 是否必选 |
|:----:|:----:|:----|:----:|
| exif | 字符串 | 获取图片详细信息   | 是 |

**请求头部**

使用公共请求头部，具体请参考[公共请求头部](http://公共请求头部)。

###  响应语法
```
HTTP/1.1 <status_code>
Content-Type: <type>
Date: <date>
Content-Length: <length>
Server: WS-web-server
x-wos-request-id: <request id>
<?xml version="1.0" encoding="UTF-8"?>
<Exif>
  ……
</Exif>
```
**响****应头部**
使用公共响应头部，具体请参考[公共响应头部](http://公共请求头部)。

**响应消息元素**

| 消息头部   | 类型 | 描述 |
|:----|:----|:----|
|    |    |    |