# complete upload

### 功能描述
1. 如果用户上传完所有的段，就可以调用合并段接口，系统将在服务端将用户指定的段合并成一个完整的对象。在执行“合并段”操作以前，用户不能下载已经上传的数据。
2. 在合并段时需要将多段上传任务初始化时记录的附加消息头信息拷贝到对象元数据中，其处理过程和普通上传对象带这些消息头的处理过程相同。
3. 已经上传的段，只要没有取消对应的多段上传任务，都要占用用户的容量配额；对应的多段上传任务“合并段”操作完成后，只有指定的多段数据占用容量配额，用户上传的其他此多段任务对应的段数据如果没有包含在“合并段”操作制定的段列表中，“合并段”完成后删除多余的段数据，且同时释放容量配额。
4. 合并完成的多段上传数据可以通过已有的下载对象接口，下载整个多段上传对象。
5. 合并完成的多段上传数据可以通过已有的删除对象接口，删除整个多段上传对象的所有分段数据，删除后不可恢复。
6. 合并完成的多段上传数据不记录整个对象的MD5作为Etag，在下载多段数据或List空间内对象看到的多段数据其Etag的生成方式为： MD5（M1M2……MN）-N，其中，Mn表示第n段的MD5值， N表示总共的段数。

### 请求语法
```
POST /BucketName/ObjectName?uploadId=uploadID HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: date
Authorization: Authorization
Content-Length: ContentLength

<CompleteMultipartUpload>
    <Part>
        <PartNumber>partNum1</PartNumber>
        <ETag>etag1</ETag>
    </Part>
    <Part>
        <PartNumber>partNum2</PartNumber>
        <ETag>etag2</ETag>
    </Part>
    <Part>
        <PartNumber>partNum3</PartNumber>
        <ETag>etag3</ETag>
    </Part>
</CompleteMultipartUpload>
```

### 请求参数
| 请求参数 | 是否必填 | 描述                    |
| :------- | :------- | :----------------------|
| uploadId      | 是       | 分段上传任务号<br>类型：字符串 |

#### 请求头部

使用[公共请求头部](http://公共请求头部)。

#### 请求主体
请求主体为XML格式内容

| 元素名称 | 描述  |
| ---- | ----- |
|CompleteMultipartUpload|合并的段列表。<br/>类型：XML。<br/>父节点：空<br/>子节点：一个或多个Part元素|
|Part|某个上传段的元素列表。<br/>类型：XML<br/>父节点：CompleteMultipart<br/>子节点：PartNumber, ETag|
|PartNumber|段号。<br/>类型：整型。<br/>父节点：Part|
|ETag|对应段的ETag值。<br/>类型：字符串。<br/>父节点：Part|

### 响应语法
```
HTTP/1.1 StatusCode
x-wos-request-id: RequestId
Date: Date
ETag: EtagValue
Content-Length: ContentLength
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<CompleteMultipartUploadResult xmlns="http://wcs.chinanetcenter.com/document">
    <Location>http://example-Bucket.Username.r1.s1.wcsapi.com/example-Object</Location>
    <Bucket>BucketName</Bucket>
    <Key>ObjectName</Key>
    <ETag>ETag</ETag>
</CompleteMultipartUploadResult>
```

#### 响应头部
使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

### 响应主体
响应主体为XML格式内容

| 元素名称 | 描述                                            |
| :---------- | :-------------------------------------------------- |
|CompleteMultipartUpload|响应容器<br/>类型：XML<br/>子节点：Location, Bucket, Key, ETag<br/>父节点：空|
|Location|合并后得到的对象的url<br/>类型：字符串<br/>父节点：ComleteMultipartUpload|
|Bucket|合并段所在的空间<br/>类型：字符串<br/>父节点：CompleteMultipartUpload|
|Key|合并得到对象的key<br/>类型：字符串<br/>父节点：CompleteMultipartUpload|
|ETag|根据各个段的ETag计算得出的结果<br/>类型：字符串<br/>父节点：CompleteMultipartUpload|


## 实例
#### 请求实例
```
POST /bucket01/myobject?
uploadId=AAAsb2FkIElEIGZvciBlbHZpbmcncyWeeS1tb3ZpZS5tMnRzIRRwbG9hZA HTTP/1.1
Host:Bucket.s3-cn-south-1.wcsapi.com
Date: Mon, 1 Nov 2019 20:34:56 GMT
Content-Length: 391
Authorization: WOA AKIAIOSFODNN7EXAMPLE:0RQf4/cRonhpaBX5sCYVf1bNRuU
=
<CompleteMultipartUpload>
    <Part>
        <PartNumber>1</PartNumber>
        <ETag>"a54357aff0632cce46d942af68356b38"</ETag>
    </Part>
    <Part>
        <PartNumber>2</PartNumber>
        <ETag>"0c78aef83f66abc1fa1e8477f296d394"</ETag>
    </Part>
    <Part>
        <PartNumber>3</PartNumber>
        <ETag>"acbd18db4cc2f85cedef654fccc4a4d8"</ETag>
    </Part>
</CompleteMultipartUpload>
```

#### 响应实例
```
 HTTP/1.1 200 OK
 x-wos-request-id: 100315319214214320170602225414pFHJIshn
 Access-Control-Allow-Origin: *
 Connection: close
 Content-Type: application/xml
 Content-Length: 301
Date: Mon, 1 Nov 2019 20:34:56 GMT

<?xml version="1.0" encoding="UTF-8" standalone="yes"?><CompleteMultipartUploadResult xmlns="http://wcs.chinanetcenter.com/document"><Bucket>bucket01</Bucket><ETag>4dc0dc31f93e3f73c52ce251fcdfcaa2-2</ETag><Key>multiobject</Key><Location>http://bucket01.s3-cn-south-1.wcsapi.com/myobject</Location></CompleteMultipartUploadResult>
```

#### 特殊错误
|Situation|Http Status|Error Code|Message|
|-----||||
|消息体Xml数据中给的etag值不对|400 Bad Request|InvalidPart|One or more of the specified parts could not be found.  The part may not have been uploaded, or the specified entity tag may not match the part's entity tag.|
|未带xml数据|400 Bad Request|InvalidRequest|You must specify at least one part|
|Xml数据格式不对，比如缺少partNumber标签等|400 Bad Request|MalformedXML|The XML you provided was not well-formed or did not validate against our published schema|
