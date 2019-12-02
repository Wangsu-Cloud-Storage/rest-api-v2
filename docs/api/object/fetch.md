# 抓取文件-函数计算

请求语法

```
POST /Bucket/ObjectName?x-wos-fetch HTTP/1.1
Host: wos.endPoint.com
Content-Length: xx
Date: Fri, 04 May 2019 03:21:12 GMT
Authorization: WOS <AccessKeyId>:<Signature>
x-wos-resourceURL=<resourceURL>
x-wos-content-md5=<md5>
x-wos-notifyURL=<notifyUrl>
x-wos-notifyMode=<notifyMode>
x-wos-withTS=<withTSorNot>
x-wos-decompressFormat=<decompressFormat>
```