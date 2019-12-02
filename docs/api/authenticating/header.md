# 在Header中包含签名

您可以在HTTP请求中带Authorization的Header来包含签名

**Authorization字段计算的方法**

```
Authorization = "WOS " + AccessKeyId + ":" + Signature
Signature = base64(hmac-sha1(AccessKeySecret,
               VERB + "\n"
               + Content-MD5 + "\n"
               + Content-Type + "\n"
               + Date + "\n"
               + CanonicalizedWOSHeaders
               + CanonicalizedResource))
```
细节分析如下：
* AccessKeySecret表示签名所需的密钥。
* VERB表示HTTP请求的Method，主要有PUT、GET、POST、HEAD、DELETE等。
* \n表示换行符。
* Content-MD5表示请求内容数据的MD5值，对消息内容（不包括头部）计算MD5值获得128比特位数字，对该数字进行base64编码得出。该请求头可用于消息合法性的检查（消息内容是否与发送时一致），例如”eB5eJF1ptWaXm4bijSPyxw==”，也可以为空。详情请参见[RFC2616 Content-MD5](https://www.ietf.org/rfc/rfc2616.txt)。
* Content-Type表示请求内容的类型，例如”application/octet-stream”，也可以为空。
* Date表示此次操作的时间，且必须为GMT格式，例如”Sun, 22 Nov 2015 08:16:38 GMT”。不能为空
* CanonicalizedWOSHeaders表示以x-wos- 为前缀的HTTP Header的字典序排列。
* CanonicalizedResource表示用户想要访问的WOS资源。不能为空

**构建CanonicalizedWOSHeaders的方法**

所有以x-wos-为前缀的HTTP Header被称为CanonicalizedWOSHeaders，它的构建方法如下：

1. 将所有以 x-wos-为前缀的HTTP请求头的名字转换成小写的形式 。例如X-WOS-Meta-Name:MetaInfo转换成x-wos-meta-name:MetaInfo。
2. 将步骤1中得到的所有HTTP请求头按照名字的字典序进行升序排列。
3. 删除请求头和内容之间分隔符两端出现的任何空格。例如x-wos-meta-name: MetaInfo转换成x-wos-meta-name:MetaInfo。
4. 将每一个请求头和内容用\n分隔符分隔拼成最后的CanonicalizedWOSHeaders。

**说明**

* CanonicalizedWOSHeaders可以为空，无需添加最后的分隔符\n。
* 如果只有一个CanonicalizedWOSHeaders，例如x-wos-meta-a\n，则需要在最后加上\n。
* 如果有多个CanonicalizedWOSHeaders，例如x-wos-meta-a:a\nx-wos-meta-b:b\nx-wos-meta-c:c\n，则需要在每一个CanonicalizedWOSHeaders之后加上\n。

**构建CanonicalizedResource的方法**

用户发送请求中想访问的WOS目标资源被称为CanonicalizedResource，它的构建方法如下：

1. 将CanonicalizedResource置为空字符串""。
2. 设置要访问的WOS资源/BucketName/ObjectName。如果仅有BucketName而没有ObjectName，则CanonicalizedResource为”/BucketName/“，如果既没有BucketName也没有ObjectName，则CanonicalizedResource为“/”。
3. 如果请求的资源包括子资源（SubResource） ，那么将所有的子资源按照字典序，从小到大排列并以&为分隔符生成子资源字符串。在CanonicalizedResource字符串尾添加?和子资源字符串。此时的CanonicalizedResource为/BucketName/ObjectName?acl&uploadId=UploadId。

**说明**

* WOS目前支持的子资源（SubResource）包括............等。
* 子资源（SubResource）有三种类型：
  * 资源标识，例如子资源中的acl、append、uploadId、symlink等，详见.......。
  * 指定返回Header字段，例如response-***，详见GetObject的Request Parameters。
  * 文件（Object）处理方式，例如x-wos-process，详见............。

**计算签名头规则**

* 签名的字符串必须为UTF-8格式。含有中文字符的签名字符串必须先进行 UTF-8 编码，再与 AccessKeySecret计算最终签名。
* 签名的方法用[RFC 2104](http://www.ietf.org/rfc/rfc2104.txt)中定义的HMAC-SHA1方法，其中Key指的是AccessKeySecret。
* Content-Type和Content-MD5在请求中不是必须的，但是如果请求需要签名验证，空值的话以换行符 \n 代替。
* 在所有非HTTP标准定义的header中，只有以x-wos-开头的header，需要加入签名字符串（如下方签名示例中的x-wos-magic则需要加入签名字符串）；其他非HTTP标准header将被WOS忽略。

**说明** 

以x-wos-开头的header在签名验证前需要符合以下规范：

* header的名字需要变成小写。
* header按字典序自小到大排序。
* 分割header name和value的冒号前后不能有空格。
* 每个header之后都有一个换行符“\n”，如果没有header，CanonicalizedWOSHeaders则设置为空。