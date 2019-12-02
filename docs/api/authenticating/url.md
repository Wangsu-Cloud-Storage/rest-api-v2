# 在URL中包含签名

除了使用Authorization Header，您还可以在URL中加入签名信息，以便将该URL转给第三方实现授权访问。

**说明** 

* 不支持同时在URL和Header中包含签名。
* PUT和GET请求都支持在URL中签名。

URL签名示例：

```
http://wos-example.wcsapi.com/wos-api.pdf?WOSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D
```
URL签名必须至少包含Signature、Expires和WOSAccessKeyId三个参数。生成签名字符串时，除Date被替换成Expires参数外，仍然包含content-type、content-md5等在Header中包含签名中定义的Header（请求中虽然仍有Date这个请求Header，但不需要将Date加入签名字符串中）。
* Expires参数的值是一个[Unix time](https://baike.baidu.com/item/unix%E6%97%B6%E9%97%B4%E6%88%B3/2078227?fr=aladdin)（自UTC时间1970年1月1号开始的秒数），用于标识该URL的超时时间。如果WOS接收到这个URL请求的时间晚于签名中包含的Expires参数时，则返回请求超时的错误码。例如：当前时间是1141889060，开发者希望创建一个60秒后自动失效的URL，则可以设置Expires时间为1141889120。
* WOSAccessKeyId 即密钥中的AccessKeyId。
* Signature表示签名信息。所有WOS支持的请求和各种Header参数，在URL中进行签名的算法和在Header中包含签名的算法基本一样。
```
Signature = urlencode(base64(hmac-sha1(AccessKeySecret,
          VERB + "\n"
          + CONTENT-MD5 + "\n"
          + CONTENT-TYPE + "\n"
          + EXPIRES + "\n"
          + CanonicalizedWOSHeaders
          + CanonicalizedResource)))
```
**说明** 

与Header中包含签名相比主要区别如下：

* 通过URL包含签名时，Header中包含签名的Date参数替换为Expires参数。
* 如果多次传入Signature、Expires或AccessKeyId，以第一次为准。
* 先验证请求时间是否晚于Expires时间，然后再验证签名。
* 将签名字符串放到URL时，须对URL进行urlencode。