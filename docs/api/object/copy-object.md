# CopyObject

### 功能介绍
为已经存在的对象创建一个副本。

注：如果在复制操作之前对象已经存在，复制操作执行之后老对象则会被新复制对象覆盖。

### 请求语法
```
PUT /<BucketName>/<DestinationObjectName> HTTP/1.1
Host: wos.<Endpoint>
Date: <date>
Authorization: Authorization
x-wos-copy-source: /<SourceBucket>/<SourceObject>
x-wos-copy-source-if-match: <etag>
x-wos-copy-source-if-none-match: <etag>
x-wos-copy-source-if-unmodified-since: <time_stamp>
```
**请求头部**
| 请求头部   | 类型   | 描述 | 是否必选 |
|:----:|:----|:----:|:----:|
| x--copy-source |    | 用来指定复制对象操作的源空间名以及源对象名。此消息头必须进行URL编码。   类型：字符串   示例：x-amz-copy-source:/src_bucket/src_object | 是 |
| x-amz-copy-source-if-match |    | 只有当源对象的Etag与此参数指定的值相等时才进行复制对象，否则返回412（precondition failed）。   类型：字符串   示例：x-amz-copy-source-if-match: etag   约束条件：此参数可与x-amz-copy-source-if-unmodified/modified-since一起使用，但不能与其它条件复制参数一起使用。 | 否 |
| x-amz-copy-source-if-none-match |    | 只有当源对象的Etag与此参数指定的值不相等时才进行复制对象操作，否则返回412（precondition failed）。   类型：字符串。   示例：x-amz-copy-source-if-none-match: etag | 否 |
| x-amz-copy-source-if-unmodified-since |    | 只有当源对象在此参数指定的时间之后没有修改过才进行复制对象操作，否则返回412（precondition failed），此参数可与x-amz-copy-source-if-match一起使用，但不能与其它条件复制参数一起使用。   类型：符合[http://www.ietf.org/rfc/rfc2616.txt](http://www.ietf.org/rfc/rfc2616.txt)规定格式的HTTP时间字符串。   示例：x-amz-copy-source-if-unmodified -since: timestamp | 否 |
| x-amz-copy-source-if-modified-since |    | 只有当源对象在此参数指定的时间之后修改过才进行复制对象操作，否则返回412（前置条件不满足），此参数可与x-amz-copy-source-if-none-match一起使用，但不能与其它条件复制参数一起使用。   类型：符合[http://www.ietf.org/rfc/rfc2616.txt](http://www.ietf.org/rfc/rfc2616.txt)规定格式的HTTP时间字符串。   示例：x-amz-copy-source-if-modified-since: time-stamp | 否 |


### 响应语法