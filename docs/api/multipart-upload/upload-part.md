# upload part

### 功能描述
1. 多段上传任务创建后，用户可以通过指定多段上传任务号，通过上传段接口为特定的任务上传段，从客户端上传新数据。同一个对象的同一个多段上传任务在上传段时，上传的顺序对后续的合并操作没有影响，也即支持多个段并发上传。
2. 上传段的编号有范围限制，其范围是[1,10000]。段编号是唯一确定一个段和合并段时所在的位置。如果上传了一个重复段编号的段，那之前上传的相同编号的段将被覆盖。段大小最小是5MB，但在进行合并段操作时，最后一个段的大小没有限制。
3. 为了确保数据在传输过程不被破坏，可以加Content-MD5请求头部，系统就会校验MD5，MD5不对时，WCS会给出出错信息。
4. 注：多段任务创建且已经上传了一个或多个段，只有合并段或取消多段任务后才会释放占用的存储空间，否则将一直占用空间不会释放。

### 请求语法
```
PUT /Bucketname/ObjectName?partNumber=partNum&uploadId=uploadId HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: date
Authorization: Authorization
Content-MD5：MD5
```

#### 请求参数
| 请求参数 | 是否必填 | 描述                    |
| :------- | :------- | :----------------------|
| partNumber      | 是       | 分段编号<br>类型：整形 |
| uploadId      | 是       | 分段上传任务号<br>类型：字符串 |


#### 请求头部

除使用公共请求头部，还可以使用如下表附加的请求头部：

| 请求头部 | 是否必填 | 描述                    |
| :------- | :------- | :----------------------|
|Content-Length|是|本次上传数据大小，以字节为单位<br/>类型：整型|
|Content-MD5|否|上传数据的128位MD5值。这个头部通过判断头部的值是否等于上传数据计算的值，用于数据完整性的校验。<br/>类型：字符串|
|Expect||当使用100-continue状态码时，客户端直到收到服务端的确认之后才会发生请求消息体。如果请求基于头部被拒绝则请求消息体不会被发送，这个头部只有在发送一个消息体时才会使用否|

#### 请求主体
请求主体为分段内容。

### 响应语法
```
HTTP/1.1 StatusCode
x-wos-request-id: RequestId
Date: Date
ETag: EtagValue
Content-Length: ContentLength
```

#### 响应头部
使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

#### 响应主体
无响应主体。

## 实例
#### 请求实例
```
PUT /bucket01/myobject?
partNumber=1&uploadId=VCVsb2FkIElEIGZvciBlbZZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZR HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Mon, 1 Nov 2019 20:34:56 GMT
Content-Length: 1048596
Authorization:WOS 14RZT432N80TGDF2Y2G2:8se2hm3YLchJhuPMDrybeITcuo0=
Content-MD5:q3q7DaS8pTI6thGbtdzSlg==
```

#### 响应实例
```
HTTP/1.1 200 OK
x-wos-request-id: 001B21A61C6C0000013403098535528C
Date: Mon, 1 Nov 2019 20:34:56 GMT
ETag: "b54357faf0632cce46e942fa68356b38"
Content-Length: 1048596
```

#### 特殊错误
|Situation|Http Status|Error Code|Message|
|-------|----|----|----|
|签名中和URL中均只带同一个参数，uploadId或partNumber|400 Bad Request |InvalidArgument|Invalid Argument|
|签名中和URL中均带了uploadId/partNumber，且顺序一致，但是顺序不正常|400 Bad Request|InvalidArgument|Invalid Argument|
|partNumer>10000|400 Bad Request|InvalidArgument|Part number must be an integer between 1 and 10000|
|单次upload的partsize>=5G|400 Bad Request|EntityTooLarge|Your proposed upload exceeds the maximum allowed object size.|