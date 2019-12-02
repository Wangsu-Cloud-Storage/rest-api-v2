# 公共Http头

本文介绍了对象存储 WOS 的公共请求头和公共响应头。

公共请求头（Common Request Headers）

| 名称   | 描述   |
|:----|:----|:----:|
| Authorization   | 请求的身份验证。匿名请求不需要带  用于认证请求者的身份合法性。  类型：字符串  默认值：无   |
| Content-Length   | RFC 2616 中定义的消息（不包含消息头）长度。所有PUT操作必须带  类型：字符串。  默认值：无。 |
| Content-Type   | 当请求的内容在正文中，资源内容的类型，例如： text/plain。  类型：字符串。  默认值：无。   |
| Content-MD5   | 消息（不包含消息头）的128位MD5摘要，需要经过Base64编码  类型：字符串  默认值：无   |
| Date   | 请 求 发 起 端 的 当前日 期 和 时 间 。 当 请 求 中 带  Authorization 时，则请求中必须定义头部 Date 或者  x-wos-date。  类型：字符串。  默认值：无。   |
| Host   | 表明主机地址。对于路径类型的请求，该值为服务  端 的 域 名 ， 会 在 域 名 中 指 定 region ， 如  username.r1/r2/r3.s1.wcsapi.com 或者为 IP 地址。对  于虚拟主机方式的请求，该值为空间名加域名，如  bucket.username.r1/r2/r3.s1.wcsapi.com  类型：字符串。  默认值：无。   |
| Expect   | 当您的请求使用 100-continue时，客户端直到收到服  务端的确认之后才会发送请求消息体。如果请求基  于头部被拒绝，则请求消息体不会被发送，这个头部  只有在发送消息体时才会使用  有效值：100-continue   |
| x-wos-date | 请 求 发 起 端 的当前 日 期 和 时 间 。 当 请 求 中 带Authorization 时，则请求中必须定义头部 Date 或者x-wos-date。如果请求中头部 Date 和 x-wos-date 同时定义，则以头部 x-wos-date 为主。类型：字符串默认值：无   |

公共响应头（Common Response Headers）

| 名称   | 描述   |
|:----|:----|:----:|
| Content-Length   | 响应消息体的字节长度  类型：字符串  默认值：无   |
| Content-Type   | 响应内容的MIME类型。例如，Content-Type:text/html; charset=utf-8  类型：字符串  默认值：无   |
| Connection   | 指明与服务器的连接是打开的还是关闭的。  类型：字符串。  有效值：keep-alive   |
| Date   | WOS 响应的日期和时间。  类型：字符串。  默认值：无。   |
| ETag   | 对象的 Hash 值，Etag只反应对象内容的变化，而不是其元数据。通过PUT Object, POST Object, Copy 操作或控制台创建的对象，Etag就是该对象数据的MD5摘要；如果是通过分段上传创建的对象，Etag就不是MD5摘要。    上传的对象或拷贝操作创建的对象，通过MD5加密后都有唯一的ETag。如果通过多段上传对象，则无论加密方法如何，MD5会拆分ETag，此类情况ETag就不是MD5的摘要。  类型：字符串。  默认值：无   |
| Server   | 创建响应的服务器名字。  类型：字符串  默认值：WOS-Web-Server   |
| x-wos-request-id   | 由 WOS 创建来唯一确定本次请求的值，可以通过该值来定位问题。  类型：字符串。  默认值：无。   |