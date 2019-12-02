# 图片鉴定

### 描述
对图片资源进行j智能鉴定，识别图片是否涉黄、涉恐或包含政治人物。

### 请求参数
公共请求头部请参考[公共参数](http://公共参数)，请求内容是一段xml格式的数据，格式如下

| 名称   | 类型   | 是否必填   | 说明   |
|:----|:----|:----|:----|
| scenes   | 字符串或数组   | 是   | 鉴定类型，有效值为porn-鉴黄,terror-暴恐,political-政治人物识别。可只传递porn鉴黄，或传递["porn","terror"]同时鉴黄和鉴恐。   |
| resources   | 字符串或数组   | 是   | 待鉴定的资源，为资源的URL。   |

### 返回参数
公共响应头部请参考[公共参数](http://公共参数)，响应内容是一段xml格式的数据，格式如下

| 名称 | 父元素   | 类型 | 说明   |
|:----|:----|:----|:----|
| results   | -   | array   | 鉴定结果   |
| image   | results   | string   | 请求鉴定的图片   |
| errMessage   | results   | string   | 错误信息，鉴定成功返回空值   |
| pornDetect   | results   | node   | 鉴黄信息，**仅type=porn时返回该节点**   |
| label   | pornDetect   | int   | 图片鉴黄结果分类；0-色情， 1-性感， 2-正常   |
| rate   | pornDetect   | float   | 介于0-1间的概率值，概率越高，鉴定结果越肯定   |
| review   | pornDetect   | boolean   | 是否需要人工复审该图片；true-需要，false-不需要   |
| -   |    |    |    |
| terrorDetect   | results   | node   | 鉴恐信息，**仅type=terror时返回该节点**   |
| label   | terrorDetect   | int   | 图片鉴恐结果分类；0-非暴恐， 1-暴恐   |
| rate   | terrorDetect   | float   | 介于0-1间的概率值，概率越高，鉴定结果越肯定   |
| review   | terrorDetect   | boolean   | 是否需要人工复审该图片；true-需要，false-不需要   |
| -   |    |    |    |
| politicalDetect   | results   | node   | 政治人物识别信息，**仅type=political时返回该节点**   |
| label   | politicalDetect   | int   | 图片政治人物识别结果分类；0-非政治人物， 1-政治人物   |
| persons   | politicalDetect   | array   | 政治人物信息   |
| name   | persons   | string   | 政治人物名字   |
| rate   | persons   | float   | 介于0-1间的概率值，概率越高，鉴定结果越肯定   |
| review   | persons   | boolean   | 是否需要人工复审该图片；true-需要，false-不需要   |

### 示例
请求示例

```
POST /image/detect
Host: xxx
<?xml version="1.0" encoding="UTF-8" ?>
	<scenes>porn</scenes>
	<scenes>terror</scenes>
	<scenes>political</scenes>
	<resources>fileUrl</resources>
```
返回示例
```
<?xml version="1.0" encoding="UTF-8" ?>
	<results>
		<image>image</image>
		<errMessage>errMessage</errMessage>
		<pornDetect>
			<label>label</label>
			<rate>rate</rate>
			<review>review</review>
		</pornDetect>
		<terrorDetect>
			<label>label</label>
			<rate>rate</rate>
			<review>review</review>
		</terrorDetect>
		<politicalDetect>
			<label>0</label>
			<persons>
				<name>name</name>
				<rate>rate</rate>
				<review>review</review>
			</persons>
		</politicalDetect>
	</results>
	<results>
		<image>image</image>
		<errMessage>errMessage</errMessage>
		<pornDetect>
			<label>label</label>
			<rate>rate</rate>
			<review>review</review>
		</pornDetect>
		<terrorDetect>
			<label>label</label>
			<rate>rate</rate>
			<review>review</review>
		</terrorDetect>
		<politicalDetect>
			<label>0</label>
			<persons>
				<name>name</name>
				<rate>rate</rate>
				<review>review</review>
			</persons>
		</politicalDetect>
	</results>
```