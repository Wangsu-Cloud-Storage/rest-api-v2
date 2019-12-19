# list multi parts

### 功能描述
1. 用户通过列举段接口查询一个任务所属的所有段信息。
2. 此接口返回最多1000个上传分片，同时默认返回分片数目也是1000个。
3. 用户可以通过max-parts参数限制返回的分片数目，如果分片数目超过1000，响应返回一个值为True的IsTruncated和NextPartNumberMarker元素。在后续的list parts请求中，可以通过将查询字符串参数part-number-marker的值设为上一次响应中NextPartNumberMarker的值来获取后续的分片列表。

### 请求语法

```
GET /BucketName/ObjectName?uploadId=uploadid&max-parts=max&part-number-marker=marker HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: date
Authorization: Authorization
```

### 请求参数
| 请求参数 | 是否必填 | 描述                    |
| :------- | :------- | :----------------------|
| uploadId  | 是  | 分段编号<br>类型：字符串 |
| max-parts | 否  | 规定在列举已上传段响应中的最大Part数目。<br>类型：字符串<br>默认值：1000 |
| part-number-marker | 否  | 指定List的起始位置，只有Part Number数目大于该参数的Part会被列出。<br>类型：字符串。<br>默认值：无 |

#### 请求头部

使用[公共请求头部](http://公共请求头部)。

#### 请求主体

无请求主体。

### 响应语法
```
HTTP/1.1 status_code
x-amz-request-id: requestId
Date: Date
Content-Length: Length
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListPartsResult xmlns=" http://wcs.chinanetcenter.com/document">
    <Bucket>BucketName</Bucket>
    <Key>object</Key>
    <UploadId>uploadid</UploadId>
    <Initiator>
        <ID>initiatorid</ID>
        <DisplayName>displayname</DisplayName>
    </Initiator>
    <Owner>
        <ID>ownerid</ID>
        <DisplayName>ownername</DisplayName>
    </Owner>
    <PartNumberMarker>partNmebermarker</PartNumberMarker>
    <NextPartNumberMarker>nextpartnumbermarker</NextPartNumberMarker>
    <MaxParts>2</MaxParts>
    <IsTruncated>true</IsTruncated>
    <Part>
        <PartNumber>partnumber</PartNumber>
        <LastModified>modifieddate</LastModified>
        <ETag>etag</ETag>
        <Size>size</Size>
    </Part>
    …
</ListPartsResult>
```

#### 响应头部

使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

#### 响应主体

响应主体为XML格式内容

| 元素名称 | 描述                                            |
| :---------- | :-------------------------------------------------- |
|ListPartsResult|保存List Part请求结果的容器。<br/>类型：XML容器<br/>子节点：Bucket, Key, UploadId, PartNumberMarker, NextPartNumberMarker, MaxParts, IsTruncated, Part.<br/>父节点：无	|
|Bucket|Bucket名称<br/>类型：字符串<br/>父节点：ListPartsResult|
|Key|Object名称<br/>类型：字符串<br/>节点：ListPartsResult|
|UploadId|Upload任务ID<br/>类型：字符串<br/>父节点：ListPartsResult|
|Initiator|Upload任务的创建者<br/>类型：XML容器<br/>子节点：ID、DisplayName<br/>父节点：ListPartsResult|
|ID|创建者的DomainID<br/>类型：字符串<br/>父节点：Initiator、Owner|
|DisplayName|创建者的名字<br/>类型：字符串<br/>父节点：Initiator、Owner|
|PartNumberMarker|本次List结果的Part Number起始位置<br/>类型：整数<br/>父节点：ListPartsResult|
|NextPartNumberMarker|如果本次没有返回全部结果，响应请求中将包含NextPartNumberMarker元素，用于标明接下来请求的PartNumberMarker的值。<br/>类型：整数<br/>父节点：ListPartsResult|
|MaxParts|返回请求中的最大Part数目,maxparts取值范围为[1,1000]。<br/>类型：整数<br/>父节点：ListPartsResult|
|IsTruncated|标明是否本次返回的List Part结果列表被截断。“true”表示本次没有返回全部结果；“false”表示本次已经返回了全部结果。<br/>类型：布尔值<br/>父节点：ListPartsResult|
|Part|保存Part信息的容器。<br/>类型：XML容器<br/>子节点：PartNumber, LastModified, ETag, Size。<br/>父节点：ListPartsResult|
|PartNumber|已上传Part的编号。<br/>类型：整型。<br/>父节点：ListPartsResult|
|LastModified|Part上传的时间。<br/>类型：日期。<br/>父节点：ListPartsResult|
|ETag|已上传Part内容的ETag。<br/>类型：字符串。<br/>父节点：ListPartsResult|
|Size|已上传Part大小。<br/>类型：整数。<br/>父节点：ListPartsResult|

### 示例

#### 请求示例

```
GET /mybucket/object01?
uploadId=XXBsb2FkIElEIGZvciBlbHZpbmcncyVcdS1tb3ZpZS5tMnRzEEEwbG9hZA&max-parts=2&part-number-marker=1 HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Mon, 1 Nov 2019 20:34:56 GMT
Authorization: WOS AKIAIOSFODNN7EXAMPLE:0RQf4/cRonhpaBX5sCYVf1bNRuU=
```

#### 响应示例

```
HTTP/1.1 200 OK
x-amz-request-id: 656c76696e6727732072657175657374
Date: Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 985
Connection: keep-alive

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListPartsResult xmlns=" http://wcs.chinanetcenter.com /document">
<Bucket>example-bucket</Bucket>
<Key>example-object</Key>
<UploadId>XXBsb2FkIElEIGZvciBlbHZpbmcncyVcdS1tb3ZpZS5tMnRzEEEwbG9hZA</UploadId>
<Initiator>
<ID> 11116a31-17b5-4fb7-9df5-b288870f11xx</ID>
<DisplayName>umat-user-11116a31-17b5-4fb7-9df5-b288870f11xx</DisplayName>
</Initiator>
<PartNumberMarker>1</PartNumberMarker>
<NextPartNumberMarker>3</NextPartNumberMarker>
<MaxParts>2</MaxParts>
<IsTruncated>true</IsTruncated>
<Part>
<PartNumber>2</PartNumber>
<LastModified>2010-11-10T20:48:34.000Z</LastModified>
<ETag>"7778aef83f66abc1fa1e8477f296d394"</ETag>
<Size>10485760</Size>
</Part>
<Part>
<PartNumber>3</PartNumber>
<LastModified>2010-11-10T20:48:33.000Z</LastModified>
<ETag>"aaaa18db4cc2f85cedef654fccc4a4x8"</ETag>
<Size>10485760</Size>
</Part>
</ListPartsResult>
```

### 模板补充说明

```
1、bucketName都写在host里
2、书写英文单词的规范
   <1>使用大驼峰
   <2>自定义头部x-wos-* 全部小写
3、文档内不要出现：“文件名”这个描述，以“对象”来描述
4、表格内容居左对齐
5、endPoint使用真实域名s3-cn-south-1.wcsapi.com来代替书写
```

### 特殊错误
|Situation|Http Status|Error Code |Message|
|-----|-----|-----|-----|
|指定不存在的uploadId，如已经complete或者abort的uploadId|404 Not Found|NoSuchUpload|The specified upload does not exist. The upload ID may be invalid, or the upload may have been aborted or completed.|
|指定的uploadId与指定的object不匹配|404 Not Found|NoSuchUpload|The specified upload does not exist. The upload ID may be invalid, or the upload may have been aborted or completed.|
|max-parts值为负数或大于1000|400 Bad Request|InvalidArgument|Argument maxParts must be an integer between 0 and 1000|
|max-parts值为非数字|400 Bad Request|InvalidArgument|Provided maxParts not an integer or within integer range.|
|part-number-marker值为非数字|400 Bad Request|InvalidArgument|Provided part-number-marker not an integer|