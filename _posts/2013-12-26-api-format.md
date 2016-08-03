---
layout: post
title: api format
date: '2013-12-26'
cateory: api
tags: [api]
---




|文件编号|版本号|拟制人/修改人|拟制/修改日期|更改理由|主要更改内容（写要点即可）|
|---|---|---|---|---|---|
#概述
本文件规定了Android|IOS 项目API需求的规范，以便实现API通用性标准，及通用性解析。

##接口说明
###接口形式
API接口请求方式为get和post
返回数据形式为json或xml

###参数命名规则
|参数标示|举例|说明|
||param|无标示：表示参数是必须的|

###附加参数
附加参数不参与签名，仅作辅助和统计功能使用。附加参数更具项目具体情况定制，也可不使用

|参数|类型|说明|
|---|---|---|
|version|String|API版本号 ，版本控制使用|

#签名规则

key为服务端的唯一字符串，用来标示某一类型客户端，也作为签名时的私钥。

		  = android_app1001888888xxxx
		= 13a052dcef103d81d21e5f434ae0913f

|参数|类型|说明|
|---|---|---|
|Success|成功或失败时的状态码|数字（一般200定义为成功）|

###登录授权规则
  


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
<?xml version="1.0" encoding="utf-8"?>
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
<?xml version="1.0" encoding="utf-8"?>
	<message>成功!</message>
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
<?xml version="1.0" encoding="utf-8"?>
	<message>成功!</message>
		<nickname>Cangol</nickname>
		<url>http://xxx/xxx</url>
		<type>1</type>
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
<?xml version="1.0" encoding="utf-8"?>
	<message>成功!</message>
		<item>
			<nickname>Cangol</nickname>
			<url>http://xxx/xxx</url>
			<type>1</type>
		</item>
		<item>
			<nickname>Cangol2</nickname>
			<url>http://xxx/xxx</url>
			<type>1</type>
		</item>
```