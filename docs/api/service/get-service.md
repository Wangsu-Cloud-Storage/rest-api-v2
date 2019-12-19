# GetService
### 功能描述
 用户可以通过请求查询自己拥有的空间列表。
### 请求语法
```
GET / HTTP/1.1
Host: s3-cn-south-1.wcsapi.com
Date: Date
Authorization:WOS <AccessKeyId>:<Signature>
```
#### 请求参数
该请求无请求参数。

#### 请求头部

使用[公共请求头部](http://公共请求头部)

#### 请求主体

无请求主体。

### 响应语法
```
HTTP/1.1 Status_Code
x-wos-request-id: Request_Id
Date: Date
Content-Type: Type
Content-Length: Length

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListAllMyBucketsResult
xmlns="http://wcs.chinanetcenter.com/document">
   <Owner>
    <ID>ID</ID>
    <DisplayName>DisplayName</DisplayName>
   </Owner>
   <Buckets>
    <Bucket>
      <Name>BucketName</Name>
      <CreationDate>Date</CreationDate>
    </Bucket>
   </Buckets>
</ListAllMyBucketsResult>
```
#### 响应头部

使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

#### 响应主体
XML格式内容
| 元素名称 | 描述  |
|:----|:----|:----|
| ListAllMyBucketsResult | 响应容器<br>类型：容器<br>子节点：Owner，Buckets  <br>父节点：无   |
| Owner | 空间拥有者信息。<br>类型：容器<br>父节点：ListAllMyBucketsResult   |
| ID | 用户Owner ID。<br>类型：字符 
| DisplayName | 空间拥有者用户名类型：字符串。父节点：ListAllMyBucketsResult.Owner   |
| Buckets | 用户所拥有空间列表容器。<br>类型：容器子节点：Bucket <br> 父节点：ListAllMyBucketsResult   |
| Bucket | 具体的空间信息。  <br>类型：容器  <br>子节点：Name, CreationDate  <br>父节点：ListAllMyBucketsResult.Buckets   |
| Name | 空间名称。<br>类型：字符串<br>父节点：ListAllMyBucketsResult.Buckets.Bucket   |
| CreationDate | 空间的创建时间。<br>类型：字符串<br>父节点：ListAllMyBucketsResult.Buckets.Bucket     |

###  示例
#### 请求示例
```
GET / HTTP/1.1
Host: s3-cn-south-1.wcsapi.com
Date: Mon, 25 Jun 2018 05:37:12 +0000
Authorization: WOS GKDF4C7Q6SI0IPGTXTJN:9HXkVQIiQKw33UEmyBI4rWrzmic= 

```

#### 响应示例

```
HTTP/1.1 200 OK
Server: WOS
x-wos-request-id: BF260000016435722C11379647A8A00A
Date: Date
Content-Type: Type
Content-Length: Length

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListAllMyBucketsResult
xmlns="http://wcs.chinanetcenter.com/document">
   <Owner>
    <ID>783fc6652cf246c096ea836694f71855</ID>
    <DisplayName>UserName1111</DisplayName>
   </Owner>
   <Buckets>
    <Bucket>
      <Name>ExampleName01</Name>
      <CreationDate>
      2018-06-21T09:15:01.032Z
      </CreationDate>
    </Bucket>
    <Bucket>
      <Name>ExampleName02</Name>
      <CreationDate>
      2018-06-21T09:15:01.032Z
      </CreationDate>
    </Bucket>
   </Buckets>
</ListAllMyBucketsResult>
```