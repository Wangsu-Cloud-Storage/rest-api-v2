# 初始化分片上传

### 功能描述
使用多段上传接口时，用户必须首先调用创建多段上传任务接口创建任务，系统会给用户返回一个全局唯一的多段上传任务号，作为任务标识。后续用户可以根据这个标识发起相关的请求，如：上传段、合并段、列举段等。同一个对象可以同时存在多个多段上传任务；每个多段上传任务在初始化时可以附加消息头信息。
*注：当初始化一个分片任务且已经上传分片，只要未complete或者abort，这些分片会一直占用存储空间不会释放。*

### 请求语法

```
POST /Bucketname/ObjectName?uploads HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: date
Authorization: Authorization
```

#### 请求参数
| 参数名称 | 是否必填 | 描述                                  |
| -------- | -------- | ------------------------------------- |
| uploads  | 是       | 表明这是多段上传任务<br/>类型：字符串 |

#### 请求头部
使用[公共请求头部](http://公共请求头部)。

#### 请求主体
无请求主体。

### 响应语法

```
HTTP/1.1 StatusCode
Content-Type: Type
Date: Date
Content-Length: ContentLength
Etag: Etag
x-wos-request-id: RequestId

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<InitiateMultipartUploadResult xmlns="http://wcs.chinanetcenter.com/document">
    <Bucket>BucketName</Bucket>
    <Key>ObjectName</Key>
    <UploadId>uploadID</UploadId>
</InitiateMultipartUploadResult>
```

#### 响应头部
使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

#### 响应主体

该请求响应消息中通过返回消息元素，返回本次多段上传任务的多段上传任务号、空间名、对象名，供后续上传段、合并段使用，元素的具体意义如下表所示:
|元素名称|描述|
|----|---|
|InitiateMultipartUploadResult|描述多段上传任务的容器<br/>类型：XML<br/>子节点：Bucket，Key，UploadId<br/>父节点：空|
|Bucket|多段上传对象所在空间的空间名<br/>类型：字符串<br/>父节点：InitiateMultipartUploadResult|
|Key|多段上传对象的key<br/>类型：字符串<br/>父节点：InitiateMultipartUploadResult|
|UploadId|多段上传id，后面进行多段上传时，利用这个id指定多段上传任务<br/>类型：字符串<br/>父节点：InitiateMultipartUploadResult|

### 实例

#### 请求实例
```
POST /bucket01/myobject?uploads HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Mon, 1 Apr 2017 20:34:56 GMT
Authorization: WOS AKIAIOSFODNN7EXAMPLE:VGhpcyBtZXNzYWdlIHNpZ25lZGGieSRlbHZpbmc=
```

#### 响应实例
```
HTTP/1.1 200 OK
x-wos-request-id: 996c76696e6727732072657175657374
Date: Mon, 1 Apr 2017 20:34:56 GMT
Content-Length: 146
Connection: keep-alive
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<InitiateMultipartUploadResult xmlns="http://wcs.chinanetcenter.com/document">
    <Bucket>bucket01</Bucket>
    <Key>objectkey</Key>
    <UploadId>DCD2FC98B4F70000013DF578ACA318E7</UploadId>
</InitiateMultipartUploadResult>
```

#### 特殊错误
|Situation|Http Status|Error Code|Message|
|-----|-----|-----|----|
|签名有带uploads，URL中没带uploads；或者URL有带uploads，签名中没带uploads|403 Foribidden|SignatureDoesNotMatch|The request signature we calculated does not match the signature|
|签名有带uploads，URL中的uploads非法（比如拼写错误）；或者URL有带uploads，签名中uploads非法（比如拼写错误）|403 Foribidden|SignatureDoesNotMatch|The request signature we calculated does not match the signature you provided.|
|签名和URL均有带uploads，但是拼写错误不一致|403 Foribidden|	SignatureDoesNotMatch|The request signature we calculated does not match the signature you provided.|