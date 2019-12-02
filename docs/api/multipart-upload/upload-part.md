# upload part

#UploadPart

标签（空格分隔）： 分片相关接口

---

- [描述](#描述)
- [请求消息](#请求消息)
  - [请求消息样式](#请求消息样式)
  - [请求消息参数](#请求消息参数)
  - [请求消息头](#请求消息头)
  - [请求消息元素](#请求消息元素)
- [响应消息](#响应消息)
  - [响应消息样式](#响应消息样式)
  - [响应消息头](#响应消息头)
  - [响应消息元素](#响应消息元素)
  - [特殊错误](#特殊错误)
- [实例](#实例)

## 描述
多段上传任务创建后，用户可以通过指定多段上传任务号，通过上传段接口为特定的任务上传段，从客户端上传新数据。同一个对象的同一个多段上传任务在上传段时，上传的顺序对后续的合并操作没有影响，也即支持多个段并发上传。
上传段的编号有范围限制，其范围是[1,10000]。段编号是唯一确定一个段和合并段时所在的位置。如果上传了一个重复段编号的段，那之前上传的相同编号的段将被覆盖。段大小最小是5MB，但在进行合并段操作时，最后一个段的大小没有限制。
为了确保数据在传输过程不被破坏，可以加Content-MD5请求头部，系统就会校验MD5，MD5不对时，WCS会给出出错信息。
<font color="red">注：多段任务创建且已经上传了一个或多个段，只有合并段或取消多段任务后才会释放占用的存储空间，否则将一直占用空间不会释放。</font>

### 请求消息样式
```
PUT /bucketname/ObjectName?partNumber=partNum&uploadId=uploadId HTTP/1.1
Host: endpoint
Date: date
Content-Length: Size
Authorization: authorization
Content-MD5：md5
```
### 请求消息参数
此接口不使用请求参数。
### 请求消息头
此接口除了使用[公共请求消息头](https://www.zybuluo.com/mdeditor#770721)，还使用以下请求头部：
|名称|描述|是否必选
|----|||
|Content-Length|本次上传数据大小，以字节为单位<br/>类型：整型|是|
|Content-MD5|上传数据的128位MD5值。这个头部通过判断头部的值是否等于上传数据计算的值，用于数据完整性的校验。<br/>类型：字符串|否|
|Expect|当使用100-continue状态码时，客户端直到收到服务端的确认之后才会发生请求消息体。如果请求基于头部被拒绝则请求消息体不会被发送，这个头部只有在发送一个消息体时才会使用|否|
### 请求消息元素
此接口中不使用请求消息元素。
## 响应消息
### 响应消息样式
```
HTTP/1.1 status_code
x-wos-request-id: request id
Date: date
ETag: etagValue
Content-Length: length
```
### 响应消息头
该接口的响应消息使用公共消息头，具体请参考[公共响应消息头](https://www.zybuluo.com/mdeditor#770721)章节
### 响应消息元素
该接口中的响应消息中不带响应消息元素。
### 特殊错误
|Situation|Http Status|Error Code|Message|
|-------|----|----|----|
|签名中和URL中均只带同一个参数，uploadId或partNumber|400 Bad Request |InvalidArgument|Invalid Argument|
|签名中和URL中均带了uploadId/partNumber，且顺序一致，但是顺序不正常|400 Bad Request|InvalidArgument|Invalid Argument|
|partNumer>10000|400 Bad Request|InvalidArgument|Part number must be an integer between 1 and 10000|
|单次upload的partsize>=5G|400 Bad Request|EntityTooLarge|Your proposed upload exceeds the maximum allowed object size.|

## 实例
**请求实例**
```
PUT /bucketname/ObjectName?
partNumber=1&uploadId=VCVsb2FkIElEIGZvciBlbZZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZR HTTP/1.1
Host: endpoint
Date: Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 1048596
Authorization:AWS 14RZT432N80TGDF2Y2G2:8se2hm3YLchJhuPMDrybeITcuo0=
Content-MD5:q3q7DaS8pTI6thGbtdzSlg==
```
**响应实例**
```
HTTP/1.1 200 OK
x-wos-request-id: 656c76696e6727732072657175657374
Date: Mon, 1 Nov 2010 20:34:56 GMT
ETag: "b54357faf0632cce46e942fa68356b38"
Content-Length: 1048596
```
