# GetBucket

###  功能描述
      列举空间内的对象。 如果用户在请求的URI里只指定了空间名，即GET /BucketName，则返回信息中会包空间内部   分或所有对象的描述信息（一次最多返回1000个对象信息）；如果用户还指定了prefix、marker、max-keys、delimiter参数中的一个或多个，则返回的对象列表将按照规定的语义返回指定的对象。

### 请求语法
```
GET /?prefix=Prefix&marker=Marker&max-keys=Max-Keys&delimiter=Delimiter&startTime=StartTime&endTime=EndTime
Host: BucketName.s3-cn-south-1.wcsapi.com
Date: Date
Authorization: WOS <AccessKeyId>:<Signature>
```
#### 请求参数


| 参数名称 | 是否必填| 描述 |
|:----|:----|:----|
| prefix | 否 | 列举以指定的字符串prefix开头的对象。可以指定前缀将空间对象分组，就像文件系统中使用文件夹一样。<br>类型：字符串。 |
| marker |否 | 列举空间内对象列表时，指定一个标识符，返回的对象列表将是按照字典顺序排序后这个标识符以后的所有对象。<br>类型：字符串。|
| max-keys | 否 | 指定返回的最大对象数，返回的对象列表将是按照字典顺序的最多前max-keys个对象，范围是[1，1000]，超出范围时，按照默认的1000进行处理。<br>类型：整型。 |
| delimiter | 否 | 用来指定将对象名按照特定字符分割的分割符。如果指定了prefix参数，按delimiter对所有对象命名进行分割，多个对象分割后prefix到第一个delimiter间都相同对象会形成一条CommonPrefixes；如果没有携带prefix参数，按delimiter对所有对象命名进行分割，多个对象分割后从对象名开始到第一个delimiter之间相同的部分形成一条CommonPrefixes。<br>类型：字符。 |
| startTime   | 否 | 文件上传起始时间，格式为精确到毫秒的时间差，如1526745600000（2015/5/20 00:00:00）     |
| endTime   | 否 | 文件上传终止时间，格式为精确到毫秒的时间差，如1527609600000（2015/5/30 00:00:00）。|
| ~~mode~~   | ~~否~~ | ~~指定列表排序方式：0代表优先列出目录下的文件；1代表优先列出目录下的文件夹。~~  ~~不指定该参数时，即按照key排序列出目录下的所有文件及子目录下的文件。~~  ~~注意：指定mode参数时，可以通过prefix参数指定查询的目录，此时prefix参数不支持模糊查询。~~  ~~未指定mode参数时，prefix参数支持模糊查询~~  <br>~~类型：~~  <br>~~有效值：1\|0~~    |


### 请求头部

使用[公共请求头部](http://公共请求头部)

 ### 请求主体

无请求主体。

### 响应语法
```
HTTP/1.1 Status_Code
Server：WOS
x-wos-request-id: Request_Id
Date: Date
Content-Type: Type
Content-Length: Length

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListBucketResult xmlns="http://wcs.chinanetcenter.com/document">
  <Name>ExampleBucket</Name>
  <Prefix/>
  <Marker/>
  <MaxKeys>MaxKeys</MaxKeys>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>Key</Key>
    <LastModified>Date</LastModified>
    <UploadTime>Date</UploadTime>
    <ETag>Etag</ETag>
    <Size>Size</Size>
    <Owner>
      <ID>ID</ID>
      <DisplayName>DisplayName</DisplayName>
    </Owner>
    <StorageClass>StorageClass</StorageClass>
  </Contents>
</ListBucketResult>
```
### 响应头部
使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

### 响应主体

XML格式内容

| 元素名称 | 描述  |
|:----|:----|
| ListBucketResult | 空间内对象列表。<br>类型：XML |
| Contents | 对象的元数据信息。类型：<br>XML<br>父节点：ListBucketResult |
| CommonPrefixes | 请求中带delimiter参数时，返回消息带CommonPrefixes分组信息。 只有定义了delimiter响应才会返回commonprefixes。CommonPrefixes包含Prefix与delimiter指定的下一次出现的字符串之间的所有（如果有）。CommonPrefixes 列出了与prefix指定目录的子目录相同的键。  <br>类型：XML<br>父节点：ListBucketResult |
| Delimiter | 请求中携带的delimiter参数。<br>类型：字符串<br>父节点：ListBucketResult |
| ETag | 对象的base64编码的128位MD5摘要。ETag是对象内容的唯一标识，可以通过该值识别对象内容是否有变化。比如上传对象时ETag为A，下载对象时ETag为B，则说明对象内容发生了变化。实际的ETag是对象的哈希值。ETag只反映变化的内容，而不是其元数据。上传的对象或拷贝操作创建的对象，通过MD5加密后都有唯一的ETag。<br>类型：字符串<br>父节点：ListBucketResult.Contents |
| ID   | 对象拥有者的用户ID。 <br> 类型：字符串 <br> 父节点：ListBucketResult.Contents.Owner   |
| DisplayName   | 对象拥有者的用户名。   |
| IsTruncated | 表明是否本次返回的ListBucketResult结果列表被截断。“true”表示本次没有返回全部结果；“false”表示本次已经返回了全部结果。<br>类型：Boolean<br>父节点：ListBucketResult |
| Key | 对象名。<br>类型：字符串<br>父节点：ListBucketResult.Contents |
| LastModified | 对象最近一次被修改的时间。<br>类型：Date<br>父节点：ListBucketResult.Contents |
| Marker | 列举对象时的起始位置。<br>类型：字符串<br>父节点：ListBucketResult |
| NextMarker   | 如果本次没有返回全部结果，响应请求中将包含此字段，用于标明本次请求列举到的最后一个对象。后续请求可以指定Marker等于该值来列举剩余的对象。  <br>类型：字符串  <br>父节点：ListBucketResult   |
| MaxKeys   | 列举时最多返回的对象个数。  <br>类型：字符串  <br>父节点：ListBucketResult   |
| Name | 本次请求的空间名。<br>类型：字符串<br>父节点：ListBucketResult |
| Owner   | 用户信息，包含用户ID和用户名。 <br> 类型：XML <br> 父节点：ListBucketResult.Contents   |
| Prefix | 对象名的前缀，表示本次请求只列举对象名能匹配该前缀的所有对象。<br>类型：字符串<br>父节点：ListBucketResult |
| Size | 对象大小。<br>类型：字符串<br>父节点：ListBucketResult.Contents |
| StorageClass   | 对象的存储类型。<br>  类型：枚举值 <br> 有效值：STANDARD \| WARM \| COLD <br> 父节点：ListBucketResult.Contents   |
| uploadTime   | 上传时间。 <br> 类型：Date  <br>父节点：ListBucketResult.Contents   |
| mineType   | 资源内容的MIME类型。<br>类型：字符串  <br>父节点：ListBucketResult.Contents   |
| expirationDate   | 文件过期时间。 <br> 类型：Date <br> 父节点：ListBucketResult.Contents   |

### 示例
#### 示例1  返回全部对象列表

##### 请求示例

```
GET / HTTP/1.1
Host: examplebucket.wos.s3-cn-east-1.wcsapi.com
Date: Wed, 12 Oct 2019 17:50:00 GMT
Authorization: WOS H4IPJX0TQTHTHEBQQCEC:iZeDESIMxBK2YODk7vIeVpyO8DI=
Content-Type: text/plain
```
##### 响应示例
```
HTTP/1.1 200 OK
Server: WOS
x-wos-request-id: BF260000016435D34E379ABD93320CB9
Content-Type: application/xml
Date: WED, 01 Jul 2019 02:23:30 GMT
Content-Length: 586

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListBucketResult xmlns="http://wcs.chinanetcenter.com/document">
  <Name>ExampleBucket</Name>
  <Prefix/>
  <Marker/>
  <MaxKeys>1000</MaxKeys>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>Object001</Key>
    <LastModified>2019-07-01T00:32:16.482Z</LastModified>
    <UploadTime>2019-06-01T00:32:16.482Z</UploadTime>
    <ETag>"2fa3bcaaec668adc5da177e67a122d7c"</ETag>
    <Size>12041</Size>
    <Owner>
      <ID>b4bf1b36d9ca43d984fbcb9491b6fce9</ID>
      <DisplayName>webfile</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
</ListBucketResult>
```
#### 示例2   指定消息参数prefix，marker，max-keys请求
##### 请求示例

```
GET /?prefix=N&marker=Ned&max-keys=2 HTTP/1.1
Host: example.wos.s3-cn-east-1.wcsapi.com
Date: Wed, 01 Oct  2019 12:00:00 GMT
Authorization:  WOS H4IPJX0TQTHTHEBQQCEC:iZeDESIMxBK2YODk7vIeVpyO8DI=
```
##### 响应示例
```
HTTP/1.1 200 OK
x-wos-request-id: 3B3C7C725673C630
Date: Wed, 01 Oct  2019 12:00:00 GMT
Content-Type: application/xml
Content-Length: 302
Connection: close
Server: WOS
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns="http://wcs.chinanetcenter.com/document">
  <Name>examplebucket</Name>
  <Prefix>N</Prefix>
  <Marker>Ned</Marker>
  <MaxKeys>2</MaxKeys>
  <IsTruncated>true</IsTruncated>
  <Contents>
    <Key>Nelson</Key>
    <LastModified>2019-10-01T12:00:00.000Z</LastModified>
    <UploadTime>2019-01-01T12:00:00.000Z</UploadTime>
    <ETag>"828ef3fdfa96f00ad9f27c383fc9ac7f"</ETag>
    <Size>5</Size>
    <StorageClass>STANDARD</StorageClass>
    <Owner>
      <ID>bcaf161ca5fb16fd081034f</ID>
      <DisplayName>webfile</DisplayName>
     </Owner>
  </Contents>
  <Contents>
    <Key>Neo</Key>
    <LastModified>2019-01T12:00:00.000Z</LastModified>
    <ETag>"828ef3fdfa96f00ad9f27c383fc9ac7f"</ETag>
    <Size>4</Size>
    <StorageClass>STANDARD</StorageClass>
     <Owner>
      <ID>bcaf1ffd86a5fb16fd081034f</ID>
      <DisplayName>webfile</DisplayName>
    </Owner>
 </Contents>
</ListBucketResult>
```
#### 示例3   指定消息参数prefix和delimiter
空间examplebucket有如下对象：

sample.jpg

photos/2019/January/sample.jpg

photos/2019/February/sample2.jpg

photos/2019/February/sample3.jpg

photos/2019/February/sample4.jpg

##### 请求示例

```
GET /?prefix=photos&delimiter=/ HTTP/1.1
Host: examplebucket.wos.s3-cn-east-1.wcsapi.com
Date: Wed, 01 Oct  2019 12:00:00 GMT
Authorization:  WOS H4IPJX0TQTHTHEBQQCEC:iZeDESIMxBK2YODk7vIeVpyO8DI=
```
##### 响应示例
```
HTTP/1.1 200 OK
x-wos-request-id: 3B3C7C725673C630
Date: Wed, 01 Oct  2019 12:00:00 GMT
Content-Type: application/xml
Content-Length: 302
Connection: close
Server: WOS


<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns="http://wcs.chinanetcenter.com/document">
  <Name>examplebucket</Name>
  <Prefix>photos</Prefix>
  <Marker/>
  <MaxKeys>1000</MaxKeys>
  <IsTruncated>false</IsTruncated>
    <CommonPrefixes>
      <Prefix>photos/</Prefix>
    </CommonPrefixes>
</ListBucketResult>   
```