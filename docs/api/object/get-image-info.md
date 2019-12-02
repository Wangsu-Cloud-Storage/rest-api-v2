# GetImageInfo

### 功能介绍
用于获取图片的基本信息，包括图片的大小，宽度，高度，色彩空间，不含拍摄信息。

### 请求语法
```
HEAD /<BucketName>/<ObjectName>?imageInfo HTTP/1.1
Host: wos.<Endpoint>
Date: <date>
Authorization: <authorization>
```
**URI参数**
| 参数名称 | 类型 | 描述 | 是否必选 |
|:----:|:----:|:----:|:----|
| imageInfo  | 字符串 | 获取图片基本信息 | 是   |

**请求头部**

使用公共请求头部，具体请参考[公共请求头部](http://公共请求头部)。

### 响应语法
```
HTTP/1.1 <status_code>
Content-Type: <type>
Date: <date>
Content-Length: <length>
Server: WS-web-server
x-wos-request-id: <request id>
<?xml version="1.0" encoding="UTF-8"?>
<ImageInfo>
  <Format>Fstring</Format>
  <Size>string</Size>
  <Width>int</Width>
  <Height>int</Height>
  <ColorModel>string</ColorModel>
</ImageInfo>
```
**响****应头部**
使用公共响应头部，具体请参考[公共响应头部](http://公共请求头部)。

**响应消息元素**

| 消息头部   | 类型 | 描述 |
|:----|:----:|:----|
| Format   | 字符串 | 表示图片的格式   |
| Size   | 字符串   | 表示图片的文件大小   |
| Width   | 字符串   | 表示图片的宽度，单位：像素（PX）   |
| Height   | 字符串   | 表示图片的高度，单位：像素（PX）   |
| ColorModel   | 字符串   | 表示图片的彩色空间，如palette16、ycbcr、RGB等   |