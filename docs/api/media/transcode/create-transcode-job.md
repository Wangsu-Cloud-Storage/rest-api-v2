# 新建转码任务

### 描述

新建转码任务

### 请求参数

公共请求头部请参考[公共参数](http://公共参数)，请求内容是一段json格式的数据，格式如下

| 参数             | 类型       | 说明         | 是否必填 |
| ---------------- | ---------- | ------------ | -------- |
| input            | ARN Object | 输入文件对象 | 是       |
| output           | ARN Object | 输出文件对象 | 是       |
| template_id      | array      | 转码模板     | 否       |
| transcode_params |            | 转码参数     | 否       |
|                  |            |              |          |
|                  |            |              |          |

注意事项：

1、 template_id和transcode_params需要填写一项，两项都填是会忽略template_id参数。

2、output如不指定对象名称，会自动生成对象名称。

3、template_id可使用数组形式指定多个，也可直接指定单个。



ARN Object说明

```
wos://bucketName/objectName
```



transcode_params说明

```
{
	"video":{"key1":"value1","key2":"value2"},
	"audio":{"key1":"value1","key2":"value2"},
	"common":{"key1":"value1","key2":"value2"}
}
```



video参数说明

| 参数   | 类型    | 说明                                                         | 是否必填 |
| ------ | ------- | ------------------------------------------------------------ | -------- |
| format | string  | 编码格式，如MP4、flv等                                       | 是       |
| r      | integer | 视频帧率，每秒显示的帧数，单位：赫兹（Hz）。<br/>常用帧率：24，25，30等，一般用默认值。 | 否       |
| vb     | string  | 视频比特率，单位：比特每秒（bit/s）。<br/>常用视频比特率：128k，1.25m，5m等。 | 否       |
| vcodec | string  | 视频编码方案，支持方案：libx264，libws265，libvpx，libtheora，libxvid等。同时支持copy参数，保持原有视频的编码方案。注：转码场景下，当vcodec指定视频分辨率时，vb也需要配置比特率，才可以成功转码，否则默认以原编码方式拷贝源视频。 |          |
| ...    |         |                                                              |          |



audio参数说明

| 参数   | 类型   | 说明                                                         | 是否必填 |
| ------ | ------ | ------------------------------------------------------------ | -------- |
| acodec | string | 音频编码方案，支持方案：libmp3lame，libfaac，libvorbis，libfdk_aac，opus等。同时支持copy参数，保持原有音频的编码方案 | 否       |
| ab     | string | 音频码率，单位：比特每秒（bit/s），常用码率：64k，128k，192k，256k，320k等。 | 否       |
| aq     | string | 音频质量，取值范围为0-9（mp3），值越小质量越高；10-500（aac），值越大质量越高。仅支持mp3和aac。不能与音频码率参数ab共用。 | 否       |
| ...    |        |                                                              |          |



common参数说明

| 参数 | 类型 | 说明 | 是否必填 |
| ---- | ---- | ---- | -------- |
|      |      |      |          |
|      |      |      |          |
|      |      |      |          |
|      |      |      |          |



### 返回参数说明

创建成功时返回

| 参数    | 类型    | 说明     |
| ------- | ------- | -------- |
| task_id | Integer | 任务ID。 |

创建失败时返回

| 参数 | 类型   | 说明       |
| ---- | ------ | ---------- |
| code | String | 错误码。   |
| msg  | String | 错误描述。 |



### 示例

请求示例

```
POST /media/transcode

{
	"input": "wos://mysourcebucket/myvideo",
	"output": "wos://mytargetbucket/myvideo",
	transcode_params:{
		"video": {
			"format": "mp4",
			"vcodec": "libx265"
		},
		"audio": {
			"acodec": "libmp3lame"
		}
	}
}
```



返回示例

```
{
	"task_id": xxx
}
```

