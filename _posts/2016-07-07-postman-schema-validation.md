---
layout: post
title: 「Json Schema 检验」测试指南
category: 技术
comments: true
---

## 目录：
 1、抓包获取接口返回的 Json 数据
 2、生成Schema
 3、测试代码示例

-------------------------------------------------------

### 1、抓包获取接口返回的 Json 数据，例如 ```/api/user/myprofile``` 返回如下
```Json
{
	"status": {
		"code": 10000,
		"message": "成功",
		"alert": "",
		"server_time": 1465955247
	},
	"data": {
		"id": 232422,
		"username": "小明安",
		"email": "sunjinming@doweidu.com",
		"enabled": 1,
		"last_login": "2016-06-14 18:12:04",
		"locked": 0,
		"expired": 0,
		"credentials_expired": 0,
		"mobile": 13524375781,
		"avatar": "http:\/\/img-agc.iqianggou.com\/a46c9930cb351e309c10db2d13dd6727",
		"balance": 2,
		"coin": 180,
		"coin_total_gain": 64228,
		"coin_rank_percent": 0.99,
		"share_code": "h0qml",
		"openim_account": "232422",
		"created_at": "2015-01-26 00:27:06",
		"updated_at": "2016-06-15 06:44:26",
		"to_redeem_order_count": 1,
		"recommend_user_coin": "2000",
		"vendor_coupon_count": 6,
		"oauthes": [{
			"oauth_user_id": "o31SljnUR-z4T8VZ0qXlHgV0SlAk",
			"type": 1,
			"access_token": "OezXcEiiBSKSxW0eoylIePyMSietbIZsvc5fEnGZjakplCp_As7jjafqQxQZR7R_AQYkBwx8oLfOk-gv0D8vkfjDURbvzJEf0kNL0u8zS4Wkm2aC1lPLTV0Xbte2jJJpbT-T_Lua8G7xAif-WK9sMA",
			"refresh_token": "OezXcEiiBSKSxW0eoylIePyMSietbIZsvc5fEnGZjakplCp_As7jjafqQxQZR7R_Trfy00QmFS-cxaJ2p7nOLjoiX7h7n1QKFVcX",
			"extra_info": {
				"openid": "oBh6ajhyAH2OUgZxel2T-GsWM7oA",
				"nickname": "孙晋明🐶",
				"sex": 1,
				"language": "zh_CN",
				"city": "Xuhui",
				"province": "Shanghai",
				"country": "CN",
				"headimgurl": "http:\/\/wx.qlogo.cn\/mmopen\/sstLNDibp2U4Q4fRzuhiaPbtLjZkNL8Ykic1gMiavZMDcw64R2IvOicwsn8BFtPicvqduqJY0iat5RG8uuBMBtB4TGfQRrVo6MicN6c7\/0",
				"unionid": "o31SljnUR-z4T8VZ0qXlHgV0SlAk"
			}
		}]
	}
}
```
### 2、使用 Json Schema 生成工具（http://jsonschema.net/#/），生成Schema
![生成Schema](https://wt-prj.oss.aliyuncs.com/12e7f2277dfc40859d61135bd3e78bec/c8a4ba95-ad99-4cb0-b4bf-5b4f4399d48b.png)

**左边窗口填入`第一步`返回的Json数据，点击【Generate Schema】，右边窗口就生成一个 Schema（```注意：生成的Schema默认所有 "property" 都是 required，所以可能需要根据接口开发者提供的信息，修改 Schema 中的 "required" 的值```）。下面是 myprofile 接口的 Schema。**

```Json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "status": {
      "type": "object",
      "properties": {
        "code": {
          "type": "integer"
        },
        "message": {
          "type": "string"
        },
        "alert": {
          "type": "string"
        },
        "server_time": {
          "type": "integer"
        }
      },
      "required": [
        "code",
        "message",
        "alert",
        "server_time"
      ]
    },
    "data": {
      "type": "object",
      "properties": {
        "id": {
          "type": "integer"
        },
        "username": {
          "type": "string"
        },
        "email": {
          "type": "string"
        },
        "enabled": {
          "type": "integer"
        },
        "last_login": {
          "type": "string"
        },
        "locked": {
          "type": "integer"
        },
        "expired": {
          "type": "integer"
        },
        "credentials_expired": {
          "type": "integer"
        },
        "mobile": {
          "type": "integer"
        },
        "avatar": {
          "type": "string"
        },
        "balance": {
          "type": "integer"
        },
        "coin": {
          "type": "integer"
        },
        "coin_total_gain": {
          "type": "integer"
        },
        "coin_rank_percent": {
          "type": "number"
        },
        "share_code": {
          "type": "string"
        },
        "openim_account": {
          "type": "string"
        },
        "created_at": {
          "type": "string"
        },
        "updated_at": {
          "type": "string"
        },
        "to_redeem_order_count": {
          "type": "integer"
        },
        "recommend_user_coin": {
          "type": "string"
        },
        "vendor_coupon_count": {
          "type": "integer"
        },
        "oauthes": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "oauth_user_id": {
                "type": "string"
              },
              "type": {
                "type": "integer"
              },
              "access_token": {
                "type": "string"
              },
              "refresh_token": {
                "type": "string"
              },
              "extra_info": {
                "type": "object",
                "properties": {
                  "openid": {
                    "type": "string"
                  },
                  "nickname": {
                    "type": "string"
                  },
                  "sex": {
                    "type": "integer"
                  },
                  "language": {
                    "type": "string"
                  },
                  "city": {
                    "type": "string"
                  },
                  "province": {
                    "type": "string"
                  },
                  "country": {
                    "type": "string"
                  },
                  "headimgurl": {
                    "type": "string"
                  },
                  "unionid": {
                    "type": "string"
                  }
                },
                "required": [
                  "openid",
                  "nickname",
                  "sex",
                  "language",
                  "city",
                  "province",
                  "country",
                  "headimgurl",
                  "unionid"
                ]
              }
            },
            "required": [
              "oauth_user_id",
              "type",
              "access_token",
              "refresh_token",
              "extra_info"
            ]
          }
        }
      },
      "required": [
        "id",
        "username",
        "email",
        "enabled",
        "last_login",
        "locked",
        "expired",
        "credentials_expired",
        "mobile",
        "avatar",
        "balance",
        "coin",
        "coin_total_gain",
        "coin_rank_percent",
        "share_code",
        "openim_account",
        "created_at",
        "updated_at",
        "to_redeem_order_count",
        "recommend_user_coin",
        "vendor_coupon_count",
        "oauthes"
      ]
    }
  },
  "required": [
    "status",
    "data"
  ]
}
```


### 3、测试代码示例

```javascript
// 定义 Schema
var myprofileSchema = {...此处是上一步生成的 Schema}

// myprofile 接口返回的 Json 数据
var myprofileResponseBody = JSON.parse(responseBody);

// 测试 myprofile 接口返回的 Json 数据是否符合 Schema，tv4.validate()返回 `falese` 或 `true`
tests["验证/api/user/myprofile的Json格式"] = tv4.validate(profileResponseBody, schema);

// 如果返回的数据不符合 Schema，打印出 Schema validation 错误信息
if (tv4.error) {
    console.log(JSON.stringify(tv4.error, null, 4));
}
```
**Schema validation 错误信息示例**
```
{
    "code": 302,
    "message": "Missing required property: tips_array",
    "dataPath": "/data/item",
    "schemaPath": "/properties/data/properties/item/required/35",
    "subErrors": null
}
```

-----------------------------------------------------------------------------

## 参考👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇
### Usage 1: Simple validation

```javascript
var valid = tv4.validate(data, schema);
```

If validation returns ```false```, then an explanation of why validation failed can be found in ```tv4.error```.

The error object will look something like:
```json
{
    "code": 0,
    "message": "Invalid type: string",
    "dataPath": "/intKey",
    "schemaPath": "/properties/intKey/type"
}
```
**更多关于 tv4，请至**👉👉👉https://github.com/geraintluff/tv4

-------------------------------------------------------------
#### References
- 用数据生成 Json Schema [工具：http://jsonschema.net/#/](http://jsonschema.net/#/)
- Json Schema 在线验证工具：http://json-schema-faker.js.org/#
- tv4项目地址：https://github.com/geraintluff/tv4
- Json Schema 简单教程：http://wiki.jikexueyuan.com/project/json/schema.html
- http://jsonschemalint.com/draft4/#
- http://www.jsonschemavalidator.net/
