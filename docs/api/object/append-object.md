# 追加上传

### 功能描述
追加上传指在存储桶中对象的末尾追加数据，如桶中不存在对象则新增一个对象。

### 请求语法

```
POST /ObjectName?append&position=Position HTTP/1.1
Host: wos.<Endpoint>
Date: date
Authorization:WOS <AccessKeyId>:<Signature>

Object content
```

### 请求参数

| 参数名称 | 是否必填 | 说明                                   |
| -------- | -------- | -------------------------------------- |
| append   | 是       | 固定值，标识为追加上传<br>类型：字符串 |
| position | 否       | 追加写的位置                           |

### 请求头部

使用[公共请求头部](http://公共请求头部)。

### 请求主体

追加写的文件内容

### 响应语法
```
HTTP/1.1 200 ok
Date：date
Etag: etag
x-wos-request-id：requestId
x-wos-next-append-position: nextAppendPosition
```

### 响应头部

除了[公共响应头部](http://公共响应头部)，还返回如下表附加的响应头部：

| 响应头部名称               | 描述                                       |
| -------------------------- | ------------------------------------------ |
| x-wos-next-append-position | 标识对象下一次追加写的位置。<br>类型：整形 |

### 响应主体
该请求无响应主体。

### 示例
#### 请求示例

```
POST /objectName?append&position=0 HTTP/1.1
Host: bucket.endpoint
Date: Sat, 03 Dec 2011 08:28:02 +0000
Authorization: WOS BF6C09F302931425E9A7:tQ+A280jUgPCAdSTuUis35T9gWI=

1024 bytes append content
```

#### 响应示例

```
HTTP/1.1 200 ok
Date：date
Etag: etag
x-wos-request-id：request-id
x-wos-next-append-position: 1024
```

### 细节分析

1. URL参数必须包含`append`，用来指定这是一个追加操作；
2. URL参数必须包含`position`，其值指定从何处进行追加。首次追加操作的`position`必须为0，后续追加操作的`position`必须是Object的当前长度，否则返回409，错误码`PositionNotEqualToLength`。发生上述错误时，用户可以通过响应头部`x-wcs-next-append-position`来得到下一次`position`，并再次进行请求；
3. 当`position`为0，如果没有同名`appendable object`，或者同名`appendable object`长度为0，则该请求成功，其他情况都认为是`PositionNotEqualToLength`；
4. 每次`append object`的长度不能超过2G，每次`append object`都会更新object的最后修改时间；
5. 在`position`值正确的情况下，对已存在的`appendable object`追加一个长度为0的内容，该操作成功，不会改变object的状态；
6. 追加操作时不允许并发追加一个对象；
7. 追加操作要求对操作Object要有写权限。