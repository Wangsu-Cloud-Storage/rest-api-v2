# delete multi upload

### 功能描述
如果用户希望取消一个多段上传任务，可以调用取消多段上传任务接口取消任务。合并段或者取消任务接口被调用后，用户不能再对任务进行上传段和列举段的操作，并且之前已经上传的分片所占用的空间会被释放。然而如果当前存在分片正在上传中，则这些分片上传不会成功。所以，有必要多次终止一个分片上传任务以完全释放分片占据的存储空间，之后用户可以通过List Parts接口确认分片列表是否为空。

### 请求语法
```
DELETE /BucketName/ObjectName?uploadId=uplaodID HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Date
Authorization: Authorization
```

#### 请求参数

| 请求参数 | 是否必填 | 描述                    |
| :------- | :------- | :----------------------|
| uploadId      | 是       | 分段上传任务号<br>类型：字符串 |

#### 请求头部

使用[公共请求头部](http://公共请求头部)。

#### 请求主体

无请求主体。

### 响应语法
```
HTTP/1.1 status_code
x-wos-request-id: request id
Date: date
Content-Length: 0
Connection: close
Server: WS-Web-Server
```

#### 响应头部
使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

#### 响应主体

无响应主体。

## 实例
#### 请求实例
```
DELETE /bucket01/myobject?
uploadId=VXBsb2FkIElEIGZvciBlbHZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZ HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Mon, 1 Nov 2019 20:34:56 GMT
Authorization: WOS AKIAIOSFODNN7EXAMPLE:0RQf3/cRonhpaBX5sCYVf1bNRuU=
```

#### 响应实例
```
HTTP/1.1 200 OK
x-wos-request-id: 996c76696e6727732072657175657374
Date: Mon, 1 Nov 2019 20:34:56 GMT
Content-Length: 0
Connection: close
```

#### 特殊错误
|Situation|Http Status|Error Code|Message|
|----|----|----|----|
|指定不存在的uploadId，如已经complete或者abort的uploadId|404 Not Found|NoSuchUpload|The specified upload does not exist. The upload ID may be invalid, or the upload may have been aborted or completed.|
