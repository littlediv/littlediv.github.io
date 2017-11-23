# 环信 服务器 curl 获取数据 #


## 使用 APP 的 client_id 和 client_secret 获取授权管理员 token ##

client_id 和 client_secret 可以在环信管理后台的 APP 详情页面看到。

Path: /{org_name}/{app_name}/token

HTTP Method: POST

URL Params: 无

Request Headers: {“Content-Type”:”application/json”}

Request Body: {“grant_type”: “client_credentials”,”client_id”: “{APP的client_id}”,”client_secret”: “{APP的client_secret}”}

Response Body:

	curl -i -H "Content-type: application/json" -X POST -d "{"grant_type":"client_credentials","client_id":"YXA6Ysu3ECPaEee6GDmvT6_jsA","client_secret":"YXA6Ys_-0_cS29r2RP1ARN4RTiRrtDI"}" "https://a1.easemob.com/1189170418115201/weatherinteraction/token"

返回数据

>{"access_token":"YWMtq6CeUEEJEee0SF3TsvR-nQAAAAAAAAAAAAAAAAAAAAFiy7cQI9oR57oYOa9
Pr-OwAgMAAAFcPgtEYwBPGgAoGUXfVWBddrydt9KgqdjLMfy5BTnLnozJaPueaH1LyQ","expires_in
":5180521,"application":"62cbb710-23da-11e7-ba18-39af4fafe3b0"}


## 注册 IM 用户[单个] ##

在 URL 指定的 org 和 APP 中创建一个新的用户，分两种模式：开放注册和授权注册。

“开放注册”模式：注册环信账号时，不用携带管理员身份认证信息；

“授权注册”模式：注册环信账号时，必须携带管理员身份认证信息。推荐使用“授权注册”，这样可以防止某些已经获取了注册 URL 和知晓注册流程的人恶意向服务器大量注册垃圾用户。

### 开放注册 ###
Path: /{org_name}/{app_name}/users
HTTP Method: POST
URL Params: 无
Request Headers: {“Content-Type”:”application/json”}
Request Body: {“username”:”${用户名}”,”password”:”${密码}”, “nickname”:”${昵称值}”}
注：创建用户时，username 和 password 是必须的，nickname 是可选的，这个 nickname 用于 iOS 推送。如果要在创建用户时设置 nickname，请求 body 是：{“username”:”jliu”,”password”:”123456”, “nickname”:”建国”} 这种形式，下面的示例不包含 nickname。批量注册时同此理。

	 curl -X POST -i "https://a1.easemob.com/1189170418115201/weatherinteraction/users" -d "{\"username\":\"jliu\",\"password\":\"123456\"}" -H "Content-Type:application/json"


返回数据

>{
  "action" : "post",
  "application" : "62cbb710-23da-11e7-ba18-39af4fafe3b0",
  "path" : "/users",
  "uri" : "https://a1.easemob.com/1189170418115201/weatherinteraction/users",
  "entities" : [ {
    "uuid" : "5cc2052a-4114-11e7-898d-e7d3a64a9896",
    "type" : "user",
    "created" : 1495694136946,
    "modified" : 1495694136946,
    "username" : "jliu",
    "activated" : true
  } ],
  "timestamp" : 1495694136950,
  "duration" : 0,
  "organization" : "1189170418115201",
  "applicationName" : "weatherinteraction"
}

### 授权注册 ###









一、get请求 

curl "http://www.baidu.com"  如果这里的URL指向的是一个文件或者一幅图都可以直接下载到本地

curl -i "http://www.baidu.com"  显示全部信息

curl -l "http://www.baidu.com" 只显示头部信息

curl -v "http://www.baidu.com" 显示get请求全过程解析

 

wget "http://www.baidu.com"也可以

 

二、post请求

curl -d "param1=value1&param2=value2" "http://www.baidu.com"

 

三、json格式的post请求

curl -l -H "Content-type: application/json" -X POST -d '{"phone":"13521389587","password":"test"}' http://domain/apis/users.json


{"error":"json_parse","timestamp":1495693952133,"duration":0,"exception":"org.co
dehaus.jackson.JsonParseException","error_description":"Unexpected character (''
' (code 39)): expected a valid value (number, String, array, object, 'true', 'fa
lse' or 'null')\n at [Source: java.io.BufferedInputStream@649d9b47; line: 1, col
umn: 2]"}  发送请求时请求体不符合标准的JSON格式，服务器无法正确解析

-d '{"phone":"13521389587","password":"test"}' -> -d "{\"phone\":\"13521389587\",\"password\":\"test\"}"