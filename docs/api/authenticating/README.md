# 访问控制

WOS通过 AccessKey ID 和 AccessKey Secret 加密的方式，对请求的发送者进行身份认证。AccessKey ID用于标识用户，AccessKey Secret 用于加密签名字符串和WOS用来验证签名字符串。

AccessKey 根据所属账号的类型分为以下两种：

* 主账号AccessKey：拥有对所属资源的完全操作权限
* IAM账号AccessKey：由主账号授权生成，拥有对指定资源限定的操作权限

 WOS收到带身份的请求时，会通过AccessKey ID找到对应的AccessKey Secret，并以相同的方法将它用于计算签名。然后，它会将计算出的签名与请求者提供的签名进行对比。如果两个签名相匹配，则正常响应请求。如果两个签名不匹配，那么请求将被拒绝，同时返回错误消息SignatureDoesNotMatch。