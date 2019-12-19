# GetObject

### 功能介绍
用于获取某个对象，返回信息包括对象内容和相关元数据信息。

说明：如果对象存储在归档存储中，在获取对象前，首先需要使用`RestoreObject`还原生成临时对象。否则，此操作将返回`InvalidObjectStateError`。有关还原对象的信息，请参阅[RestoreObject](http://RestoreObject)接口。

### 请求语法

```
GET /Key?Query HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Date
Authorization: Authorization
If-Match: IfMatch
If-Modified-Since: IfModifiedSince
If-None-Match: IfNoneMatch
If-Unmodified-Since: IfUnmodifiedSince
Range: Range
```

#### 请求参数

GET操作获取对象内容时，允许用户通过请求参数的设置对一些响应头部值进行重写，支持重写的头部有：`Content-Type`、`Content-Language`、`Expires`、`Cache-Control`、`Content-Disposition`以及`Content-Encoding`。具体请求参数说明如下表所示：

| 参数名称 | 是否必填 | 描述 |
|:----|:----|:----|
| response-content-type | 否  | 重写响应中的`Content-Type`头。<br>类型：字符串 |
| response-expires | 否  | 重写响应中的`Expires`头。<br/>类型：字符串 |
| response-content-language | 否       | 重写响应中的`Content-Language`头。<br/>类型：字符串 |
| response-cache-control | 否       | 重写响应中的`Cache-Control`头。<br/>类型：字符串 |
| response-content-disposition | 否  | 重写响应中的`Content-Disposition`头。<br/>类型：字符串 |
| response-content-encoding | 否  | 重写响应中的```Content-Encoding```头。<br/>类型：字符串 |

**请求头部**

除使用公共请求头部，还可以使用如下表附加的请求头部：

| 请求头部   | 是否必填 | 描述 |
|:----|:----|:----|
| If-Match | 否  | 如果对象的ETag和请求中指定的ETag相同，则返回对象内容，否则返回`412 precondition failed`。<br/>类型：字符串 |
| If-Modified-Since | 否  | 如果对象的修改时间在这个值之后，返回对象内容，否则返回`304 not modified`。<br/>类型：字符串 |
| If-Unmodified-Since | 否  | 如果对象的修改时间在这个值之前，返回对象内容，否则返回`412 precondition failed`<br/>类型：字符串 |
| If-None-Match | 否  | 如果对象的ETag和请求中指定的ETag不相同，则返回对象内容，否则返回`304 not modified`。<br/>类型：字符串 |
| Range | 否  | 获取指定范围的对象内容。如果Range不合法则忽略此字段获取整个对象。<br>类型：字符串<br>格式：`bytes=byte_range`<br>示例：`bytes=0-4`或 `bytes=512-1024` |

**请求主体**

无请求主体

### **响应语法**


```
HTTP/1.1 StatusCode
Content-Type: Type
Date: Date
Content-Length: ContentLength
Etag: Etag
Last-Modified: LastModified
x-wos-request-id: RequestId
x-wos-object-type: Appendable
x-wos-next-append-position: NextAppendPosition

<Object Content>
```

#### 响应头部

除了[公共响应头部](公共响应头部)，还返回如下表附加的响应头部：

| 响应头部   | 描述 |
|:----|:----|
| x-wos-object-type | 追加上传的对象响应时返回该头部，标识对象为追加上传的对象。<br>类型：固定字符串 |
| x-wos-next-append-position   | 追加上传的对象响应时返回该头部，标识对象下一次追加写的位置。<br>类型：整型 |

#### 响应主体

返回对象二进制内容

### 示例

#### 请求示例

```
GET /test HTTP/1.1
Host: bucket.s3-cn-south-1.wcsapi.com
Date: Sat, 03 Dec 2011 08:28:02 +0000
Authorization: WOS BF6C09F302931425E9A7:tQ+A280jUgPCAdSTuUis35T9gWI=
```

#### 响应示例

```
HTTP/1.1 200 OK
x-wos-request-id: 001B21A61C6C0000013403098535528C
ETag: "507e3fff69b69bf57d303e807448560b"
Last-Modified: Sat, 03 Dec 2011 08:25:46 GMT
Accept-Ranges: bytes
Content-Length: 30
Content-Type: binary/octet-stream
Date: Sat, 03 Dec 2011 08:28:02 GMT
[30 bytes of object data]
```