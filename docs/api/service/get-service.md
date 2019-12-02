# GetService

###       **功能介绍**
       用户可以通过请求查询自己拥有的空间列表。

###    **   请求语法**
```
GET / HTTP/1.1
Host: wos.<Endpoint>
Date: date
Authorization:WOS <AccessKeyId>:<Signature>
```
**请求参数**
该请求无请求参数。

**请求头部**

       该请求使用公共请求头部。

   **   请求消息元素**

### **      响应语法**
```
HTTP/1.1 status_code
2. x-wos-request-id: request id
3. Date: date
4. Content-Type: type
5. Content-Length: length
6.
7. <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
8. <ListAllMyBucketsResult
xmlns="http://wcs.chinanetcenter.com/document">
9. <Owner>
10. <ID>id</ID>
11. <DisplayName>name</DisplayName>
12. </Owner>
13. <Buckets>
14. <Bucket>
15. <Name>bucketName</Name>
16. <CreationDate>date</CreationDate>
17. </Bucket>
18. ...
19. </Buckets>
20. </ListAllMyBucketsResult>
```
      ** 响应头部**

       该请求响应公共头部。

      ** 响应消息元素**



| **元素名称**   | **描述**   |
|:----:|:----|:----:|
| ListAllMyBucketsResult | 响应容器类型：容器子节点：Owner，Buckets  父节点：无   |
| Owner | 空间拥有者信息。类型：容器父节点：ListAllMyBucketsResult   |
| ID | 用户Owner ID。类型：字符串。父节点：ListAllMyBucketsResult.Owner   |
| DisplayName | 空间拥有者用户名类型：字符串。父节点：ListAllMyBucketsResult.Owner   |
| Buckets | 用户所拥有空间列表容器。类型：容器子节点：Bucket  父节点：ListAllMyBucketsResult   |
| Bucket | 具体的空间信息。  类型：容器  子节点：Name, CreationDate  父节点：ListAllMyBucketsResult.Buckets   |
| Name | 空间名称。类型：字符串父节点：ListAllMyBucketsResult.Buckets.Bucket   |
| CreationDate | 空间的创建时间。类型：字符串父节点：ListAllMyBucketsResult.Buckets.Bucket     |

### **      示例**