# WOS访问域名

每个存储空间（Bucket）会有一个默认的访问域名。可以使用此域名访问您在 WOS 里的资源。

本文介绍访问域名的构成规则、区域与Endpoint的对应关系以及使用方式。

* 访问域名的构成规则

除了GetService以外，其他API请求使用的域名都是带有指定Bucket的三级域名。

访问域名结构：BucketName.Endpoint。BucketName为存储空间名称，Endpoint为WOS服务终端节点，格式：s3-区域.wcsapi.com。

WOS已开通区域和Endpoint的关系表

| 区域   | 英文表示   | Endpoint   | 支持HTTPS   |
|:----|:----|:----|:----|
| 华东一区(扬州)   | cn-south-1   | s3-cn-south-1.wcsapi.com   |    |
| 华东三区(宿迁)   | cn-east-1   | s3-cn-east-1.wcsapi.com   |    |
| 华北一区(北京)   | cn-north-1   | s3-cn-north-1.wcsapi.com   |    |

通过外网访问

通过内网访问？