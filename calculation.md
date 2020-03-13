# 自动测算

标签： 神数 自动    测算 运程

---

## 项目背景
在以往自动测算的基础上额外新增2020年运程，并增加额外收费，达到营收的目的。

## 数据库设计
详细见xlxd库

|数据表|备注|
|---|---|
|user|总用户表|
|product|产品表|
|order|订单表|
|log_pay|支付日志表|
**备注：自动测算的产品分类暂时为0**

## 接口文档
|名称|地址|请求方式|
|---|---|---|
|[获取部分运程接口](#before)|automatic/fortuneGuide|POST|
|[解锁查看详细报告接口](#pay)|automatic/getFortune|POST|
|[获取完整运程接口](#fortune)|automatic/fortune|POST|

## 接口说明

><span id="before">获取部分运程接口</span>

请求地址
>https://m.me-tn.com/automatic/fortuneGuide.html

请求方式
>POST

请求参数

|参数|类型|必填|说明|
|---|---|---|---|
|month|int|必填|月数|

响应数据

- 当status=0时，表示成功
```
{
    "status": 0,
    "msg": "成功",
    "data": {
        "header": [
            "https://www.me-tn.com/upload/fortune/header/01.png"
        ],
        "price": "0.01"
    },
    "dialog": ""
}
```
- 当status=1时，表示失败，返回具体错误信息
```
{
    "status": 1,
    "msg": "月数必填",
    "data": "",
    "dialog": ""
}
```

><span id="pay">解锁查看详细报告接口</span>

请求地址
>https://m.me-tn.com/automatic/getFortune.html

请求方式
>POST

请求参数

|参数|类型|必填|说明|
|---|---|---|---|
|name|string|必填|用户姓名|
|sex|int|必填|用户性别|
|birthday|timestamp|必填|用户阳历生日时间戳|
|phone|string|必填|用户手机号|

响应数据

- 当status=0时，表示成功
```
{
    "status": 0,
    "msg": "成功",
    "data": {
        "url": "https://wx.tenpay.com/cgi-bin/mmpayweb-bin/checkmweb?prepay_id=wx14145508942257aa51c83df61632851100&package=3239774308"
    },
    "dialog": ""
}
```
- 当status=1时，表示失败，返回具体错误信息
```
{
    "status": 1,
    "msg": "姓名或性别或出生日期或手机号必填",
    "data": "",
    "dialog": ""
}
```

- 当status=2时，表示已经支付过
```
{
    "status": 2,
    "msg": "已支付",
    "data": "",
    "dialog": ""
}
```

><span id="fortune">获取完整运程接口</span>

请求地址
>https://m.me-tn.com/automatic/fortune.html

请求方式
>POST

请求参数

|参数|类型|必填|说明|
|---|---|---|---|
|month|int|必填|用户月神数|
|phone|string|必填|用户手机号|

响应数据

- 当status=0时，表示成功
```
{
    "status": 0,
    "msg": "成功",
    "data": {
        "header": [
            "https://www.me-tn.com/upload/fortune/header/01.png"
        ],
        "body": [
            "https://www.me-tn.com/upload/fortune/body/01_01png",
            "https://www.me-tn.com/upload/fortune/body/01_02png",
            "https://www.me-tn.com/upload/fortune/body/01_03png",
            "https://www.me-tn.com/upload/fortune/body/01_04png",
            "https://www.me-tn.com/upload/fortune/body/01_05png",
            "https://www.me-tn.com/upload/fortune/body/01_06png"
        ],
        "footer": [
            "https://www.me-tn.com/upload/fortune/footer/01_01png",
            "https://www.me-tn.com/upload/fortune/footer/01_02png",
            "https://www.me-tn.com/upload/fortune/footer/01_03png",
            "https://www.me-tn.com/upload/fortune/footer/01_04png",
            "https://www.me-tn.com/upload/fortune/footer/01_05png"
        ]
    },
    "dialog": ""
}
```
- 当status=1时，表示该用户没完成支付订单，前端需要重定向
```
{
    "status": 1,
    "msg": "未支付",
    "data": "",
    "dialog": ""
}
```

## 流程图
![lOoZKU](https://s2.ax1x.com/2020/01/15/lOoZKU.png)
