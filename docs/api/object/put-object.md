# PutObject

### 功能介绍
### 请求语法
```
PUT /<BucketName>/<ObjectName> HTTP/1.1
Host: wos.<Endpoint>
Content-Type: <type>
Content-Length: <length>
Date: <date>
Authorization: <authorization>
x-wos-delete-after-days: <delete after days>
<object Content>
```
**请求头部**
除使用公共请求头部，还可以使用如下表附加的消息头部：

| 请求头部   | 类型 | 描述 | 是否必选 |
|:----|:----|:----|:----:|:----|
| x-wos-delete-after-days   | 整型   | 设置文件的保存期限，单位：天。过期之后对象会被自动删除。  示例：x-wos-expires:3  注：0表示尽快删除，上传文件时建议不配置为0。   | 否   |

**请求消息元素**

不使用请求消息元素，在消息体中携带的是对象的二进制数据。

### 响应语法
```
HTTP/1.1 <status_code>
Content-Type: <type>
Date: <date>
Content-Length: <length>
Etag:<etag>
Server: WS-web-server
x-wos-request-id: <request id>
```
**响应头部**
使用公共响应头部，具体请参考[公共响应头部](http://公共请求头部)。