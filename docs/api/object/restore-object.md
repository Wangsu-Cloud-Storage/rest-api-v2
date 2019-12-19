# restore object

### 功能描述
对应归档存储类型的文件，需要先直接解冻操作后才能访问文件。

### 请求语法

```
POST /objectName?restore / HTTP/1.1
Host: Bucket.s3-cn-south-1.wcsapi.com
Date: Date
Authorization:WOS <AccessKeyId>:<Signature>

<RestoreRequest>
   <Days>2</Days>
</RestoreRequest>
```

#### 请求参数

无请求参数。

#### 请求头部

使用[公共请求头部](http://公共请求头部)。

#### 请求主体

请求主体为xml格式的解冻信息，参数包含：
| 参数名称 | 是否必填 | 描述 |
|:----|:----|:----|
| RestoreRequest | 是  | 解冻信息 |
| RestoreDaysRequest | 是  | 解冻后文件有效期，最小为1天。<br>类型：整型<br>父节点: RestoreRequest|

### 响应语法
```
HTTP/1.1 202 Accepted
```

#### 响应头部

使用公共响应头部，具体请参考[公共响应头部](http://公共响应头部)。

#### 响应主体

无响应主体。

### 示例

#### 请求示例

```
POST /objectName?restore
Host: bucket.s3-cn-south-1.wcsapi.com
Date: Sat, 03 Dec 2011 08:28:02 +0000
Authorization: WOS BF6C09F302931425E9A7:tQ+A280jUgPCAdSTuUis35T9gWI=

<RestoreRequest>
   <Days>2</Days>
</RestoreRequest>
```

#### 响应示例

```
HTTP/1.1 202 Accepted
x-wos-request-id: 001B21A61C6C0000013403098535528C
Content-Length: 30
Date: Sat, 03 Dec 2011 08:28:02 GMT
```