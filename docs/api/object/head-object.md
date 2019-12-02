# HeadObject

### 功能介绍
用于获取对象元数据，不返回对象内容。

### 请求语法
```
HEAD /<BucketName>/<ObjectName> HTTP/1.1
Host: wos.<Endpoint>
Date: <date>
Authorization: <authorization>
```
**请求头部**
除使用公共请求头部，还可以使用如下表附加的消息头部：

| 请求头部   | 类型 | 描述 | 是否必选 |
|:----:|:----:|:----:|:----:|
| If-Match | 字符串 | 如果对象的ETag和请求中指定的ETag相同，则返回对象元数据，否则的话返回412（precondition failed）。 | 否 |
| If-Modified-Since | 字符串 | 如果对象的修改时间在这个值之后，返回对象元数据；否则返回304(not modified)。 | 否 |
| If-Unmodified-Since | 字符串 | 如果对象的修改时间在这个值之前，返回对象元数据；否则返回412（precondition failed）） | 否 |
| If-None-Match | 字符串 | 如果对象的ETag和请求中指定的ETag不相同，则返回对象元数据，否则的话返回304（not modified）。 | 否 |
| ~~Range~~   | ~~字符串~~   | ~~获取指定范围的对象内容~~~~。如果Range不合法则忽略此字段获取整个对象。~~  ~~格式：bytes=byte_range~~  ~~示例：bytes=0-4或 bytes=512-1024~~   | ~~否~~   |

### 响应语法
```
HTTP/1.1 <status_code>
Content-Type: <type>
Date: <date>
Content-Length: <length>
Etag: <etag>
Last-Modified: <time>
Server: WS-web-server
x-wos-request-id: <request id>
x-wos-storage-class: <storage class>
x-wos-object-type: <object type>
x-wos-next-append-positon: <next append positon>
x-wos-expiration: <expiration>
x-wos-fsize: <fsize>
```
**响****应头部**

除了公共响应头部，还返回如下表附加的消息头部：

| 响应头部   | 类型 | 描述 |
|:----:|:----:|:----|
| x-wos-storage-class | 字符串 | 表示对象的存储类别，分别为：标准存储类型（Standard）、低频访问存储类型（Infrequent Access）、归档存储类型（Archive）。   |
| x-wos-object-type   | 字符串   | 表示对象的类型。  通过PutObject上传的对象类型为Normal。  通过AppendObject上传的对象类型为Appendable  通过MultipartUpload上传的对象类型为Multipart。   |
| x-wos-next-append-positon   | 整型   | 对于Appendable类型的对象会返回此头部，指明下一次请求应当提供的position。   |
| x-wos-expiration   | 字符串   | 对象过期时间   |
| x-wos-fsize   | 整型   | 对象大小，单位：字节（Byte）   |
| Access-Control-Allow-Origin   | 字符串   | 当对象所在的Bucket配置了CORS规则，如果请求的Origin满足指定的CORS规则时会在响应中包含这个Origin。   |
| Access-Control-Allow-Methods   | 字符串   | 当对象所在的Bucket配置了CORS规则，如果请求的Access-Control-Request-Method满足指定的CORS规则时会在响应中包含允许的Methods。   |
| Access-Control-Max-Age   | 整型   | 当对象所在的Bucket配置了CORS规则，如果请求满足Bucket配置的CORS规则时会在响应中包含MaxAgeSeconds。   |
| Access-Control-Allow-Headers   | 字符串   | 当对象所在的Bucket配置了CORS规则，如果请求满足指定的CORS规则时会在响应中包含这些Headers。   |
| Access-Control-Expose-Headers   | 字符串   | 表示允许访问客户端JavaScript程序的headers列表。当Object所在的Bucket配置了CORS规则，如果请求满足指定的CORS规则时会在响应中包含ExposeHeader。   |