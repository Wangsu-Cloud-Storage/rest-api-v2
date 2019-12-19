# HeadObject

### 功能介绍
用于获取对象元数据，不返回对象内容。

### 请求语法
```
HEAD /Key  HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Date
Authorization: Authorization
```
#### 请求参数

无请求参数。

#### 请求头部

除使用[公共请求头部](公共请求头部)，还可以使用如下表附加的请求头部：

| 请求头部   | 是否必填 | 描述 |
|:-----|:-----|:-----|
| If-Match | 字符串 | 如果对象的ETag和请求中指定的ETag相同，则返回对象元数据，否则返回412（precondition failed）。<br>类型：字符串 |
| If-Modified-Since | 否 | 如果对象的修改时间在这个值之后，返回对象元数据，否则返回304(not modified)。<br/>类型：字符串 |
| If-Unmodified-Since | 否 | 如果对象的修改时间在这个值之前，返回对象元数据；否则返回412（precondition failed））<br/>类型：字符串 |
| If-None-Match | 否 | 如果对象的ETag和请求中指定的ETag不相同，则返回对象元数据，否则返回304（not modified）。<br/>类型：字符串 |
| ~~Range~~   | 否  | ~~获取指定范围的对象内容~~~~。如果Range不合法则忽略此字段获取整个对象。~~  ~~格式：bytes=byte_range~~  ~~示例：bytes=0-4或 bytes=512-1024~~   |

#### 请求主体

无请求主体

### 响应语法

```
HTTP/1.1  StatusCode
Content-Type: Type
Date: Date
Content-Length: Length
Etag: Etag
Last-Modified: Time
Server: WS-web-server
x-wos-request-id: RequestId
x-wos-storage-class: StorageClass
x-wos-object-type: ObjectType
x-wos-next-append-positon: NextAppendPositon
x-wos-expiration: <expiration>
x-wos-fsize: Fsize
```
#### 响应头部

除了[公共响应头部](公共响应头部)，还返回如下表附加的响应头部：

| 响应头部   | 描述 |
|:-----|:----|
| x-wos-storage-class | 表示对象的存储类别，分别为：标准存储（Standard）、低频访问存储（Infrequent Access）、归档存储（Archive）。<br>类型：字符串 |
| x-wos-object-type   | 表示对象的类型。通过PutObject上传的对象类型为Normal，通过AppendObject上传的对象类型为Appendable，通过MultipartUpload上传的对象类型为Multipart。<br/>类型：字符串 |
| x-wos-next-append-positon   | 对于Appendable类型的对象会返回此头部，指明下一次请求应当提供的position。<br/>类型：整型 |
| x-wos-expiration   | 表示对象过期时间。当对象或者对象所属空间设置了lifecycle，用expiry-date和rule-id两个键值对描述对象的详细过期信息；当对象不是通过lifecycle功能设置的过期时间，仅用expiry-date键值对描述对象的详细过期信息。若对象未设置过期时间，不显示该头域。<br/>类型：字符串<br>格式：expiry-date=“date”,rule-id="rule"或者expiry-date=“date” |
| x-wos-fsize   | 对象大小，单位：字节（Byte）<br/>类型：整型 |
| Access-Control-Allow-Origin   | 当对象所在的Bucket配置了CORS规则，如果请求的Origin满足指定的CORS规则时会在响应中包含这个Origin。<br/>类型：字符串 |
| Access-Control-Allow-Methods   | 当对象所在的Bucket配置了CORS规则，如果请求的Access-Control-Request-Method满足指定的CORS规则时会在响应中包含允许的Methods。<br/>类型：字符串 |
| Access-Control-Max-Age   | 当对象所在的Bucket配置了CORS规则，如果请求满足Bucket配置的CORS规则时会在响应中包含MaxAgeSeconds。<br/>类型：整型 |
| Access-Control-Allow-Headers   | 当对象所在的Bucket配置了CORS规则，如果请求满足指定的CORS规则时会在响应中包含这些Headers。<br/>类型：字符串 |
| Access-Control-Expose-Headers   | 表示允许访问客户端JavaScript程序的headers列表。当Object所在的Bucket配置了CORS规则，如果请求满足指定的CORS规则时会在响应中包含ExposeHeader。<br/>类型：字符串 |

#### 响应主体

无响应主体

### 示例

#### 请求示例

```
HEAD /test HTTP/1.1
Host: bucket.s3-cn-south-1.wcsapi.com 
Date: Sat, 03 Dec 2011 08:28:02 +0000
Authorization: WOS BF6C09F302931425E9A7:tQ+A280jUgPCAdSTuUis35T9gWI=
```

#### 响应示例

```
HTTP/1.1 200 OK                 
x-wos-request-id: 001B21A61C6C0000013403373811529D  
ETag: "fba9dede5f27731c9771645a3986332            
Last-Modified: Sun, 1 Jan 2006 12:00:00 GMT            
8"            
Content-Length: 434234            
Content-Type: text/plain            
Date: Wed, 28 Oct 2009 22:32:00 GMT           
Server: WS-web-server
x-wos-storage-class: Standard
x-wos-object-type: Normal
x-wos-expiration: <expiration>
x-wos-fsize: Fsize
```