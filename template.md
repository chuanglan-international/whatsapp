# 创建WhatsApp模板

# 1.接口

## 1.1.请求方式：post

/intwa-api/whatsapp/template/submit

## 1.2.参数释义：

| 参数            | 参数类型 | 参数释义                                                     | 是否必填                         | 示例                                                         |
| --------------- | -------- | ------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| accountName     | String   | 用户API帐号                                                  | 是                               | I7116157                                                     |
| name            | String   | 模板名称，自定义                                             | 是                               | OTP_TEMP                                                     |
| category        | Integer  | 短信类型 1-营销,4-验证码                                     | 是                               | 1                                                            |
| messageLanguage | String   | 模板语言                                                     | 是                               | en                                                           |
| headerType      | Integer  | 消息头部1-None，2-Text，3-Image, 4-Video, 5-Document         | 是                               | 1                                                            |
| caption         | String   | 消息标题                                                     | 否                               | welcome                                                      |
| messageBody     | String   | 消息主体，也就是发送内容,如果是可变参数，参数用{{1}}表示，其中数字可变，代表第几个参数 | 是                               | Your code is {{1}}                                           |
| footer          | String   | 页脚，如果没有可以不传值                                     | 否                               | Thanks                                                       |
| buttonType      | Integer  | 响应类型（针对消息客户可以回复对应操作， 0-无按钮,1-快捷回复,2-响应按钮） | 否                               | 1                                                            |
| buttons         | List     | 响应按钮 集合                                                | 否（如果buttonType非0，则必填）  |                                                              |
| headerExample   | map      | 请求头变量示例                                               | 否(如果请求头中含有变量，则必填) | {"header_handle":["https://scontent.whatsapp.net/v/t61.29466-34/346690475_963468361631965_8621223592915055299_n.png?ccb=1-7&_nc_sid=57045b&_nc_ohc=4h6G9vAI77MAX8acITg&_nc_ht=scontent.whatsapp.net&edm=AIJs65cEAAAA&oh=01_AdRIyuECOPcniA6SZZkF87S7GR0MTcRww0tCHZe-JaJFRg&oe=64939A67"]} |
| bodyExample     | map      | 请求体变量示例                                               | 否(如果请求体中含有变量，则必填) | {"body_text":[["OTP","Video SMS"]]}                          |
| wabaId          | String   | wabaId                                                       | 必填                             | 121009624329909                                              |

buttons的参数释义

| actionType    | Integer | 按钮动作类型 1-拨打电话,2-打开连接                          | 否（如果button是2，即响应按钮，那么需要填） | 1                                    |
| ------------- | ------- | ----------------------------------------------------------- | ------------------------------------------- | ------------------------------------ |
| label         | String  | 按钮文字                                                    | 是                                          | reply                                |
| phone         | String  | 电话号码，格式**+86136XXXXXXXX**                            | 否                                          | +86136XXXXXXXX                       |
| targetUrl     | String  | 响应按钮的访问链接**必须以http://或者****https****://开头** | 否                                          | https://www.baidu.com                |
| buttonExample | list    | 请求按钮变量示例                                            | 否(请求按钮中含有变量时则必填)              | [https://www.chuanglan.com/document] |

## 1.3  示例json

```JSON
{
    "accountName": "IW2578654",
    "name": "otpModel1",
    "category": 1,
    "messageLanguage": "en",
    "headerType": 2,
    "caption": "OTP Message",
    "messageBody": "Your code for {{1}} is {{2}}.Valid in 5 minutes.",
    "buttonType": 2,
    "buttons": [
        {
            "actionType": 1,
            "label": "call us",
            "phone": "+86185xxxxxxxx"
        },
        {
            "actionType": 2,
            "label": "search",
            "targetUrl": "https://www.baidu.com"
        }
    ]
}
```

# 2.示例json请求以及展示效果

## 2.1 纯验证码

### 2.1.1 json

```JSON
{
  "accountName": "IW2578654",
  "name": "otp",
  "category": 2,
  "messageLanguage": "en",
  "headerType": 2,
  "messageBody": "Your login code for {{1}} is {{2}}.Valid in 5 minutes. ",
  "footer": "test_3f0986645dd1",
   
}
```

### 2.1.2模板效果

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=YjQ1MmQ0MWMxYjE0ZjQzNTIwNzhmNjNlYzQxOTE2NzZfQ1JRcE5tZFJPQlpHUTROaElYM2pCVXdvNlVVdTJ6a0FfVG9rZW46T1dma2JHczFjb2VuckN4MFhIR2Mxdzh2bjVmXzE2ODYxMDQxMTI6MTY4NjEwNzcxMl9WNA)

## 2.2带header、footer、快捷回复

### 2.2.1 json

```JSON
{
    "accountName": "IW2578654",
    "name": "demo",
    "category": 1,
    "messageLanguage": "en",
    "headerType": 2,
    "caption": "OTP",
    "messageBody": "Your code for {{1}} is {{2}}.Valid in 5 minutes.",
    "footer": "footer",
    "buttonType": 1,
    "buttons": [
        {      
            "label": "yes"           
        },
        {           
            "label": "no"         
        }
    ]
}
```

### 2.2.2模板效果

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=MzRmNWFhN2U5NjEwMTU2YmI2YzVjZGY3MzY4MTViNWVfNnFCTVFsdERjdDhyR1dabzBFNjNMa3haNVNsTTJJSXpfVG9rZW46UDN3VWJBTndkbzFUaUF4M3hlQWNvNVFkblFmXzE2ODYxMDQxMTI6MTY4NjEwNzcxMl9WNA)

## 2.3带header、footer、响应按钮

### 2.3.1 json

```JSON
{
    "accountName": "IW2578654",
    "name": "demo",
    "category": 1,
    "messageLanguage": "en",
    "headerType": 2,
    "caption": "OTP",
    "messageBody": "Your code for {{1}} is {{2}}.Valid in 5 minutes.",
    "footer": "footer",
    "buttonType": 2,
    "buttons": [
       {
            "actionType": 1,
            "label": "call us",
            "phone": "+86185xxxxxxxx"
        },
        {
            "actionType": 2,
            "label": "search",
            "targetUrl": "https://www.baidu.com"
        }
    ]
}
```

### 2.3.2 模板效果

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=OWIxZDc2ZTI1OTY3NWVhYzdlZWQ5ZGVlZmE2NTQ1NGZfak5kMjRNbjJKVG90SlhucWFQOEYzZkJRQ2VYcnZ2YVVfVG9rZW46VnRhRGJxQklRb0hYRlJ4SWZhcGN4aXVnbnFlXzE2ODYxMDQxMTI6MTY4NjEwNzcxMl9WNA)

## 2.4带变量参数

### 2.4.1 json

```JSON
{
    "accountName": "IW2267527",
    "name": "transland_marketing_promotion_8",
    "category": "1",
    "messageLanguage": "zh_CN",
    "headerType":2,
    "caption": "Transland API Account {{1}} Opened",
    "messageBody": "Dear customer {{1}}，your SMS API account {{2}} is avaliable, please rember the secret of this account. The secret is sent via email.",
    "footer": "Have a nice sms experience.",
    "buttonType":2,
    "headerExample": {
        "header_text": [
            "II2024089"
        ]
    },
    "bodyExample": {
        "body_text": [
            [
                "Tom",
                "II2024089"
            ]
        ]
    },
    "wabaId": "121009624329108",
    "buttons": [
        {
            "actionType": 1,
            "label": "Call Us",
            "phone": "+8615121041046"
        },
        {
            "actionType": 2,
            "label": "Websit",
            "targetUrl": "https://www.chuanglan.com/{{1}}",
            "buttonExample": [
                "https://www.chuanglan.com/document"
            ]
        }
    ]
}
```

### 2.4.2 模板效果

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=NjZkYWJlNjQxYmQ4OWIwMDJjZWNkYmViYmQzMjE2ODdfcnF1NVAwcEF6UmpOdkhFbGdsZG9oTHR2ZlhZdUNycHRfVG9rZW46TGxhdGIzOWUybzBLWlB4Yms0WmNPdzc1bkxlXzE2ODYxMDQxMTI6MTY4NjEwNzcxMl9WNA)