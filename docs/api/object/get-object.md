# GetObject

### 功能介绍
用于获取某个对象，返回信息包括对象内容和相关元数据信息。

说明：如果对象存储在归档存储中，在获取对象前，首先需要使用RestoreObject还原生成临时文件。否则，此操作将返回InvalidObjectStateError。有关还原对象的信息，请参阅XXX（待补充链接跳转）。

### 请求语法
```
GET /<BucketName>/<ObjectName>?response-content-type=<response-content-type>&response-expires=<response-expires>&response-content-language=<response-content-language>&response-cache-control=<response-cache-control>&response-content-disposition=<response-content-disposition>&response-content-encoding=<response-content-encoding> HTTP/1.1
Host: wos.<endPoint>
Date: <date>
Authorization: <authorization>
If-Match: <IfMatch>
If-Modified-Since: <IfModifiedSince>
If-None-Match: <IfNoneMatch>
If-Unmodified-Since: <IfUnmodifiedSince>
Range: <Range>
```
**URI参数**
| 参数名称 | 类型   | 描述 | 是否必选 |
|:----:|:----|:----:|:----:|
| response-content-type | 字符串   | 重写响应中的Content-Type头。 | 否 |
| response-expires | 字符串   | 重写响应中的Expires头。 | 否 |
| response-content-language | 字符串   | 重写响应中的Content-Language头。 | 否   |
| response-cache-control | 字符串   | 重写响应中的Cache-Control头。 | 否 |
| response-content-disposition | 字符串   | 重写响应中的Content-Disposition头。 | 否 |
| response-content-encoding | 字符串   | 重写响应中的Content-Encoding头。 | 否 |

**请求头部**

除使用公共请求头部，还可以使用如下表附加的消息头部：

| 请求头部   | 类型   | 描述 | 是否必选 |
|:----:|:----|:----:|:----:|
| If-Match | 字符串   | 如果对象的ETag和请求中指定的ETag相同，则返回对象内容，否则的话返回412（precondition failed）。 | 否 |
| If-Modified-Since | 字符串   | 如果对象的修改时间在这个值之后，返回对象内容；否则返回304(not modified)。 | 否 |
| If-Unmodified-Since | 字符串   | 如果对象的修改时间在这个值之前，返回对象内容；否则返回412（precondition failed）） | 否 |
| If-None-Match | 字符串   | 如果对象的ETag和请求中指定的ETag不相同，则返回对象内容，否则的话返回304（not modified）。 | 否 |
| Range | 字符串   | 获取指定范围的对象内容。如果Range不合法则忽略此字段获取整个对象。格式：bytes=byte_range示例：bytes=0-4或 bytes=512-1024   | 否 |

**请求内容 **

无

### **响应语法**
```
HTTP/1.1 <status_code>
Content-Type: <type>
Date: <date>
Content-Length: <length>
Etag: <etag>
Last-Modified: <time>
Server: WS-web-server
x-wos-request-id: <request id>
<Object Content>
```
**响应头部**

使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

对于追加上传的对象响应时返回如下两个头部：

```
x-wos-object-type: Appendable
x-wos-next-append-position: <Content-Length int64>
```
| 响应头部   | 类型 | 描述 |
|:----:|:----:|:----|
| x-wos-object-type | 固定字符串 | 标识对象为追加上传的对象   |
| x-wos-next-append-position   | 整型   | 标识对象下一次追加写的位置   |