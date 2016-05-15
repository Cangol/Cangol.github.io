---
layout: post
title: api format
date: '2013-12-26'
cateory: api
tags: [api]
---


|文件编号|版本号|拟制人/修改人|拟制/修改日期|更改理由|主要更改内容（写要点即可）|
|---|---|---|---|---|---||Y_D_02|V1.0|Cangol|2013/12/26|无|初版||Y_D_02|V2.0|Cangol|2015/01/26|无|修正|注1：每次更改归档文件（指归档发布数据库）时，需填写此表。注2：文件第一次归档时，“更改理由”、“主要更改内容”栏写“无”。
#概述
本文件规定了Android|IOS 项目API需求的规范，以便实现API通用性标准，及通用性解析。本文件适用于Android|IOS项目。

##接口说明
###接口形式
API接口请求方式为get和post开发时所有接口(部分涉及file的接口除外)需支持get方式，以便调试和联调和接口测试；上线发布时接口关闭get方式，全部采用post方式。   
返回数据形式为json或xml

###参数命名规则
|参数标示|举例|说明||---|---|---|
||param|无标示：表示参数是必须的||*|*param|星号：表示参数是可选的||?|?param|问号：表示参数是未确定的（开发中会出现）|

###附加参数
附加参数不参与签名，仅作辅助和统计功能使用。附加参数更具项目具体情况定制，也可不使用

|参数|类型|说明|
|---|---|---|
|version|String|API版本号 ，版本控制使用||device_id|String|设备ID (open-uuid)||platform|String|平台（IOS|Android…）平台控制使用||channel|String|渠道 (Google|baidu|91|appstore…)渠道控制使用||app_version|String|App版本号||os_version|String|操作系统版本号||app_id|String|应用编号：10|

#签名规则

key为服务端的唯一字符串，用来标示某一类型客户端，也作为签名时的私钥。sign为签名值，服务端收到后按相同的规则进行签名，即可验证参数的有效性。规则：将key值+所有参数（不包含辅助参数）的值按照参数名的字典序排列相加。在将值做MD5操作得到32位字符串即为签名值
	例如：	某API有参数aid=1001 b=888888 c= d=xxxx	私钥key= android_app
		  = android_app1001888888xxxx	sign=MD5(params)
		= 13a052dcef103d81d21e5f434ae0913f###结果规则

|参数|类型|说明|
|---|---|---|
|Success|成功或失败时的状态码|数字（一般200定义为成功）||Message|成功或失败时的消息文本|文本，接口返回失败时必须||Result|返回的数据(有时为空)|返回的数据对象或对象数组||Page|接口分页(非分页接口无此项)||

###登录授权规则如果API涉及用户登录或授权等操作，采用如下登录验证规则： 
  * 用户登录成功后获取用户授权的token* 其他需登录状态的接口凭借参数token作为登录授权。* 用户授权的token有有效期，有效期建议为7天或30天，失效后客户端需重新登录。* 客户端可凭借token实现自动登录（提供token验证有效接口）* 用户授权token可采用多点登录（不同设备不同token，即用户可多设备登录）或单点登录方式（用户只有一个有效token，多个设备只容许一个设备有效登录，其他设备离线）* 客户端只需记录用户的登录用户名和token，不需记录密码。


##接口文档规范

* 名称：接口名称
* 说明：接口描述
* 地址：http://xxx/xxx
* 请求方式：GET / POST
* 请求参数：

### 返回失败：  

```json
{ 
	"success": 30001,
	"message": "key 不能为空!"
}
```
```xml
<?xml version="1.0" encoding="utf-8"?><data>	<success>30001</success>	<message>key 不能为空!</message></data>
```
### 返回成功：

* 无实体返回

```json
{ 
	"success": 200,
	"message": "成功!"
}
```

```xml
<?xml version="1.0" encoding="utf-8"?><data>	<success>200</success>
	<message>成功!</message></data>
```

* 单实体返回

```json
{ 
	"success": 200,
	"message": "成功!"，
	"result": {
        "id": 10001,
        "nickname": "Cangol",
        "email": "123456@qq.com",
        "url": "http://xxx/xxx",
        "type": "1"
    }
}
```
```xml
<?xml version="1.0" encoding="utf-8"?><data>	<success>200</success>
	<message>成功!</message>	<result>		<id>10001</id>
		<nickname>Cangol</nickname>		<email>123456@qq.com</email>
		<url>http://xxx/xxx</url>
		<type>1</type>	</result></data>
```
* 多实体返回

```json
{ 
	"success": 200,
	"message": "成功!"，
	"result": [
				{
			        "id": 10001,
			        "nickname": "Cangol",
			        "email": "123456@qq.com",
			        "url": "http://xxx/xxx",
			        "type": "1"
			    },
			    {
			        "id": 10002,
			        "nickname": "Cangol2",
			        "email": "123456@qq.com",
			        "url": "http://xxx/xxx",
			        "type": "1"
			    },
			    ...
    ]
}
```
```xml
<?xml version="1.0" encoding="utf-8"?><data>	<success>200</success>
	<message>成功!</message>	<result>
		<item>			<id>10001</id>
			<nickname>Cangol</nickname>			<email>123456@qq.com</email>
			<url>http://xxx/xxx</url>
			<type>1</type>
		</item>
		<item>			<id>10002</id>
			<nickname>Cangol2</nickname>			<email>123456@qq.com</email>
			<url>http://xxx/xxx</url>
			<type>1</type>
		</item>	</result></data>
```