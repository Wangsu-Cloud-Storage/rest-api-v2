# 视频内容分析

## 创建任务
### 描述
  1. 发送视频内容分析请求
  2. 视频内容分析使用后台异步处理的方式
  3. 请求成功仅返回标识任务的唯一标识jobId。
### 请求参数
公共请求头部请参考[公共参数](http://公共参数)，请求内容是一段xml格式的数据，格式如下

| 名称   | 类型   | 是否必填   | 说明   |
|:----|:----|:----|:----|
| resources   | 字符串或数组   | 是   | 待识别的资源，为资源的URL。   |

### 返回参数
公共响应头部请参考[公共参数](http://公共参数)，响应内容是一段xml格式的数据，格式如下

| 名称 | 类型   | 说明 |
|:----|:----|:----|
| jobId   | 字符串   | 任务id，请妥善保存，用于后续任务结果查询   |

### 示例
请求示例

```
POST /vca
Host: xxx
<?xml version="1.0" encoding="UTF-8" ?>
	<resources>fileUrl</resources>
```
返回示例
```
<?xml version="1.0" encoding="UTF-8" ?>
	<jobId>xxx</jobId>
```
## 查询任务
### 示例
请求示例

```
GET /vca/search?jobId=xxx
Host: xxx
```
返回示例
```
<?xml version="1.0" encoding="UTF-8" ?>
	<jobId>jobId</jobId>
	<code>3</code>
	<desc></desc>
	<results>
		<type>scenario</type>
		<result>
			<attribute>会议室</attribute>
			<confidence>45.67</confidence>
		</result>
	</results>
	<results>
		<type>entity</type>
		<result>
			<attribute>马克杯</attribute>
			<confidence>78.65</confidence>
		</result>
	</results>
	<results>
		<type>figure</type>
		<result>
			<attribute>张三</attribute>
			<confidence>72.11</confidence>
		</result>
	</results>
	<results>
		<type>location</type>
		<result>
			<attribute>厦门</attribute>
			<confidence>88.88</confidence>
		</result>
	</results>
	<results>
		<type>keyword</type>
		<result>
			<attribute>上海</attribute>
			<confidence>94.25</confidence>
		</result>
	</results>
	<results>
		<type>logo</type>
		<result>
			<attribute>网宿</attribute>
			<confidence>94.25</confidence>
		</result>
	</results>
```