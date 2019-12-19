# 概览

本文介绍对象存储WOS提供的API

* 关于Service的操作
| API   | 描述   |
|:----|:----|
| GetService   | 查询自己拥有的空间列表   |

* 关于Bucket的操作
| API   | 描述   |
|:----|:----|
| GetBucket   | 列举空间内的对象   |

* 关于Object的操作
| API   | 描述   |
|:----|:----|
| GetObject   | 获取某个对象   |
| HeadObject   | 获取对象元数据   |
| GetImageInfo   | 获取图片的基本信息，包括图片的大小，宽度，高度，色彩空间，不含拍摄信息。   |
| GetExif   | 获取图片的详细信息，包括图像信息和拍摄信息。   |
| GetAvinfo   |    |
| PutObject   | 上传一个文件 |
| CopyObject   | 为已经存在的对象创建一个副本。   |
| DeleteObject   | 删除一个文件 |

* 关于Multipart Upload的操作
| API   | 描述   |
|:----|:----|
| InitiateMultipartUpload | 使用多段上传时，创建多段上传任务 |
| UploadPart | 为特定的任务上传段 |
| CompleteMultipartUpload | 将用户指定的段合并成一个完整的对象 |
| AbortMultipartUpload | 取消一个多段上传任务 |

- 关于数据处理的操作

| API  | 描述 |
| ---- | ---- |
|      |      |
|      |      |
|      |      |

