# delete multi upload

# AbortMultipartUpload

标签（空格分隔）： 分片相关接口

---
- [描述](#描述)
- [请求消息](#请求消息)
  - [请求消息样式](#请求消息样式)
  - [请求消息参数](#请求消息参数)
  - [请求消息头](#请求消息头)
  - [请求消息元素](#请求消息元素)
- [响应消息](#响应消息)
  - [响应消息样式](#响应消息样式)
  - [响应消息头](#响应消息头)
  - [响应消息元素](#响应消息元素)
  - [特殊错误](#特殊错误)
- [实例](#实例)

## 描述
    如果用户希望取消一个多段上传任务，可以调用取消多段上传任务接口取消任务。合并段或者取消任务接口被调用后，用户不能再对任务进行上传段和列举段的操作，并且之前已经上传的分片所占用的空间会被释放。然而如果当前存在分片正在上传中，则这些分片上传不会成功。所以，有必要多次终止一个分片上传任务以完全释放分片占据的存储空间，之后用户可以通过List Parts接口确认分片列表是否为空。
## 请求消息
### 请求消息样式
```
DELETE /bucketname/ObjectName?uploadId=uplaodID HTTP/1.1
Host: endpoint
Date: date
Authorization: authorization
```
### 请求消息参数
此接口不使用消息参数。
### 请求消息头
此接口使用[公共请求消息头](https://www.zybuluo.com/mdeditor#770721)。
### 请求消息元素
此接口请求消息中不使用请求消息元素。
## 响应消息
### 响应消息样式
```
HTTP/1.1 status_code
x-wos-request-id: request id
Date: date
Content-Length: 0
Connection: close
Server: WS-Web-Server
```
### 响应消息头
此接口请求的响应消息使用[公共响应消息头](https://www.zybuluo.com/mdeditor#770721)。
### 响应消息元素
此接口的响应消息不带响应消息元素。
### 特殊错误
|Situation|Http Status|Error Code|Message|
|----|||
|指定不存在的uploadId，如已经complete或者abort的uploadId|404 Not Found|NoSuchUpload|The specified upload does not exist. The upload ID may be invalid, or the upload may have been aborted or completed.|
## 实例
**请求实例**
```
DELETE /example-bucket/example-object?
uploadId=VXBsb2FkIElEIGZvciBlbHZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZ HTTP/1.1
Host:endpoint
Date: Mon, 1 Nov 2010 20:34:56 GMT
Authorization: AWS AKIAIOSFODNN7EXAMPLE:0RQf3/cRonhpaBX5sCYVf1bNRuU=
```
**响应实例**
```
HTTP/1.1 200 OK
x-wos-request-id: 996c76696e6727732072657175657374
Date: Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 0
Connection: close
Server: WS-Web-Server
```





