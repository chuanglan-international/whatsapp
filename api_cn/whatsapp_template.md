1.  WhatsApp模板API接口

## 1.1 文件上传

Path： /intwa-api/whatsapp/uploadFile

**Content-Type:** multipart/form-data

**Method：** POST

接口描述：WhatsApp模板支持多媒体，多媒体模板需先上传多媒体文件

请求参数

| 名称   | 类型   | 是否必须 | 默认值 | 备注 | 其他信息                                                                        |
| :--- | :--- | :--- | :-- | :- | :-------------------------------------------------------------------------- |
| file | File | 必须   |     | 文件 | 文件的 MIME 类型。有效值为：application/pdf、image/jpeg、image/jpg、image/png 和 video/mp4 |

返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息        |
| :------ | :------ | :--- | :-- | :- | :---------- |
| code    | integer | 必须   |     |    | 0：成功<br/>其他：失败  |
| data    | object  | 必须   |     |    | 返回文件句柄内容    |
| message | string  | 必须   |     |    | code对于的状态描述 |

&#x20;请求示例

```json
{
    "file":"file"
}
```

&#x20;返回示例

```json
{
    "code": "0",
    "message": "Success",
    "data": "4::aW1hZ2UvanBlZw==:ARZDIOWLhcxHAg-swqoSVjgf5pQFQGM7XvqANssarookTBgIbJz9OwAlsfEsnia073wZviAOJEaWf6rnZqVa_Aoh9rVbphjjCzTHtIAGdP-RZg:e:1695539910:1399631517488334:100092285319469:ARaUDRGLmJ_OYV4HWkM"
}
```

## 1.2 模板创建

**Path：** /intwa-api/whatsapp/template/submit

**Content-Type:** application/json

**Method：** POST

**接口描述：**

请求参数

| 名称                | 类型        | 是否必须 | 备注                                                                    | 其他信息                                                                                                |
| :---------------- | :-------- | :--- | :-------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| accountName       | string    | 必须   | 用户api账号                                                               |                                                                                                     |
| wabaId            | string    | 必须   | WhatsApp商业账户唯一标识                                                      |                                                                                                     |
| name              | string    | 必须   | 模板名称(eg\:limited\_time\_offer\_tuscan\_getaway\_2023)                 | 由小写字母、数字、下划线组合,字段限制为512个字符.                                                                         |
| category          | string    | 必须   | 模板类型                                                                  | 1-MARKETING,2-AUTHENTICATION,3-UTILITY                                                              |
| messageLanguage   | string    | 必须   | 模板语言(eg\:en)                                                          | 模板支持的语言请参考附录2                                                                                       |
| headerType        | integer   | 必须   | 是否支持多媒体,默认1                                                           | 1-None,2-Text,3-Image,4-Video,5-Document                                                            |
| caption           | string    | 非必须  | 页眉内容                                                                  | 字段限制为字符(多媒体模板时，该字段为空),动参格式{{1}}                                                                     |
| messageBody       | string    | 非必须  | 正文内容                                                                  | 字段限制为1024个字符.动参格式{{1}}{{2}},前后按序插入                                                                  |
| footer            | string    | 非必须  | 页脚内容                                                                  | 字段限制为60字符(AUTHENTICATION模板时,该字段表示过期时间,非必填,过期时间限制90分钟内)                                              |
| buttons           | object\[] | 非必须  | 按钮内容                                                                  | 按钮总数不超过10个                                                                                          |
| ├─ actionType     | integer   | 非必须  | 按钮类型                                                                  | 1-quick reply,2-visit website,3-call phone number,4-copy code(category为2时),5-auto fill(category为2时) |
| ├─ label          | string    | 非必须  | 按钮文本                                                                  |                                                                                                     |
| ├─ phoneArea      | string    | 非必须  | 号码区号(eg:86)                                                           |                                                                                                     |
| ├─ phone          | string    | 非必须  | 号码(eg:18877776666)                                                    |                                                                                                     |
| ├─ packageName    | string    | 非必须  | 应用包名                                                                  | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ sigNatureHash  | string    | 非必须  | 应用哈希散列                                                                | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ autoFill       | string    | 非必须  | 自动填充                                                                  | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ targetUrl      | string    | 非必须  | 网址访问(eg："<https://www.chuanglan.com/>{{1}}")                          | 只支持一个动参，格式{{1}}                                                                                     |
| ├─ buttonExample  | list      | 非必须  | 按钮变量示例(eg："buttonExample": \["<https://www.chuanglan.com/whatsApp>"]) | 网址访问含有变量时必填,变量内容为完整网址                                                                               |
| headerExample     | object\[] | 非必须  | header(caption)变量示例                                                   |                                                                                                     |
| ├─ header\_handle | list      | 非必须  | 多媒体模板变量(eg："header\_handle": \["4::axxxx"])                           | 多媒体模板时，填写文件上传返回时句柄内容                                                                                |
| ├─ header\_text   | list      | 非必须  | 纯文本模板变量(eg："header\_text": \["创蓝云智"])                                 | 文本动参模板变量,动参个数限制1                                                                                    |
| bodyExample       | object\[] | 非必须  | body(messageBody)变量示例                                                 |                                                                                                     |
| ├─ body\_text     | list      | 非必须  | body变量(eg: "body\_text":\[\["variable1","variable2"]])                | 变量示例与变量按序对应，变量{{1}}对应variable1，以此类推                                                                 |
| safetyAdvice      | integer   | 非必须  | AUTHENTICATION模板是否添加安全建议(0:不添加 1:添加)                                  | 选填,未添加显示：\*{{1}}\* 是你的验证码。添加后显示：\*{{1}}\* 是你的验证码。为安全起见，请不要分享这组验证码。                                  |

&#x20;返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息       |
| :------ | :------ | :--- | :-- | :- | :--------- |
| code    | integer | 必须   |     |    | 0：成功其他：失败 |
| data    | object  | 必须   |     |    |            |
| message | string  | 必须   |     |    |            |

### 请求示例

#### MARKETING模板(无页眉无动参无按钮)

*   请求参数

    ```json
     {
        "accountName": "account",
        "wabaId": "wabaId",
        "name": "transland_marketing_promotion_base",
        "category": "1",
        "messageLanguage": "en",
        "headerType":1,
        "messageBody": "Inheriting century-old classic formula and unique craft production, so that you can share the purest traditional taste in the Mid-Autumn Festival",
        "footer": "Not interested? Tap Stop promotions"
    }
    ```

#### MARKETING模板(文本/动参/按钮)

*   请求参数

    ```json
    {
        "accountName": "account",
        "name": "transland_marketing_promotion_01",
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
        "wabaId": "wabaId",
        "buttons": [
            {
                "actionType": 3,
                "label": "Call Us",
                "phoneArea": "86",
                "phone": "15121041046"
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


#### MARKETING模板(媒体/动参/按钮)

*   请求参数

    ```json
    {
        "accountName": "account",
        "name": "mid_autumn_festival_marketing_image_01",
        "category": "1",
        "messageLanguage": "en",
        "headerType": 3,
        "caption": "",
        "messageBody": "The Mid-Autumn Festival is coming, are you worried about giving gifts? Let us inspire you! We have packages {{1}} and {{2}} for you",
        "footer": "Not interested? Tap Stop promotions.",
        "headerExample": {
            "header_handle": [
                "4::aW1hZ2UvanBlZw==:ARZdvf84bxn60M8VkyubKEh5vM4c-9u40kaXmqloKSNN6CuPGnhd_4nKBEykyPp_lvZ47T6VUaV3vLnib90WHEYR1qLzQjRey5zRBvOxI735uA:e:1697801044:1399631517488334:100092285319469:ARZVM3hq_PCWscQSGqc"
            ]
        },
        "bodyExample": {
            "body_text": [
                [
                    "Fruit mooncake",
                    "Almond mooncake"
                ]
            ]
        },
        "wabaId": "wabaId",
        "buttons": [
            {
                "actionType": 1,
                "label": "Looking forward to you"
            },
            {
                "actionType": 3,
                "label": "Call Us",
                "phoneArea": "86",
                "phone": "15121041046"
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


#### AUTHENTICATION模板

请求参数

```json
{
    "accountName": "account",
    "name": "authentication_code_0001",
    "category": "2",
    "messageLanguage": "zh_CN",
    "headerType": 0,
    "footer": "10", 
    "buttons": [
        {
            "actionType": "4",
            "label": "复制验证码"
        }
    ],
    "wabaId": "wabaId"
}
```



## 1.3 模板修改

**Path：** /intwa-api/whatsapp/template/update

**Content-Type:** application/json

**Method：** POST

**接口描述：**

请求参数

| 名称                | 类型        | 是否必须 | 备注                                                                    | 其他信息                                                                                                |
| :---------------- | :-------- | :--- | :-------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| accountName       | string    | 必须   | 用户api账号                                                               |                                                                                                     |
| wabaId            | string    | 必须   | WhatsApp商业账户唯一标识                                                      |                                                                                                     |
| name              | string    | 必须   | 模板名称(eg\:limited\_time\_offer\_tuscan\_getaway\_2023)                 | 由小写字母、数字、下划线组合,字段限制为512个字符.                                                                         |
| templateId        | string    | 必须   | 模板id                                                                  |                                                                                                     |
| category          | string    | 必须   | 模板类型                                                                  | 1-MARKETING,2-AUTHENTICATION,3-UTILITY                                                              |
| messageLanguage   | string    | 必须   | 模板语言(eg\:en)                                                          | 模板支持的语言请参考附录2                                                                                       |
| headerType        | integer   | 必须   | 是否支持多媒体,默认1                                                           | 1-None,2-Text,3-Image,4-Video,5-Document                                                            |
| caption           | string    | 非必须  | 页眉内容                                                                  | 字段限制为字符(多媒体模板时，该字段为空),动参格式{{1}}                                                                     |
| messageBody       | string    | 非必须  | 正文内容                                                                  | 字段限制为1024个字符.动参格式{{1}}{{2}},前后按序插入                                                                  |
| footer            | string    | 非必须  | 页脚内容                                                                  | 字段限制为60字符(AUTHENTICATION模板时,该字段表示过期时间,非必填,过期时间限制90分钟内)                                              |
| buttons           | object\[] | 非必须  | 按钮内容                                                                  | 按钮总数不超过10个                                                                                          |
| ├─ actionType     | integer   | 非必须  | 按钮类型                                                                  | 1-quick reply,2-visit website,3-call phone number,4-copy code(category为2时),5-auto fill(category为2时) |
| ├─ label          | string    | 非必须  | 按钮文本                                                                  |                                                                                                     |
| ├─ phoneArea      | string    | 非必须  | 号码区号(eg:86)                                                           |                                                                                                     |
| ├─ phone          | string    | 非必须  | 号码(eg:18877776666)                                                    |                                                                                                     |
| ├─ packageName    | string    | 非必须  | 应用包名                                                                  | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ sigNatureHash  | string    | 非必须  | 应用哈希散列                                                                | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ autoFill       | string    | 非必须  | 自动填充                                                                  | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ targetUrl      | string    | 非必须  | 网址访问(eg："<https://www.chuanglan.com/>{{1}}")                          | 只支持一个动参，格式{{1}}                                                                                     |
| ├─ buttonExample  | list      | 非必须  | 按钮变量示例(eg："buttonExample": \["<https://www.chuanglan.com/whatsApp>"]) | 网址访问含有变量时必填,变量内容为完整网址                                                                               |
| headerExample     | object\[] | 非必须  | header(caption)变量示例                                                   |                                                                                                     |
| ├─ header\_handle | list      | 非必须  | 多媒体模板变量(eg："header\_handle": \["4::axxxx"])                           | 多媒体模板时，填写文件上传返回时句柄内容                                                                                |
| ├─ header\_text   | list      | 非必须  | 纯文本模板变量(eg："header\_text": \["创蓝云智"])                                 | 文本动参模板变量,动参个数限制1                                                                                    |
| bodyExample       | object\[] | 非必须  | body(messageBody)变量示例                                                 |                                                                                                     |
| ├─ body\_text     | list      | 非必须  | body变量(eg: "body\_text":\[\["variable1","variable2"]])                | 变量示例与变量按序对应，变量{{1}}对应variable1，以此类推                                                                 |
| safetyAdvice      | integer   | 非必须  | AUTHENTICATION模板是否添加安全建议(0:不添加 1:添加)                                  | 选填,未添加显示：\*{{1}}\* 是你的验证码。添加后显示：\*{{1}}\* 是你的验证码。为安全起见，请不要分享这组验证码。                                  |

&#x20;返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息       |
| :------ | :------ | :--- | :-- | :- | :--------- |
| code    | integer | 必须   |     |    | 0：成功其他：失败 |
| data    | object  | 必须   |     |    |            |
| message | string  | 必须   |     |    |            |

## 1.4 模板查询

**Path：** /intwa-api/whatsapp/template/list

**Content-Type:** application/json

**Method：** POST

**接口描述：**

请求参数

| 名称              | 类型     | 是否必须 | 备注                                                    | 其他信息                                   |
| :-------------- | :----- | :--- | :---------------------------------------------------- | :------------------------------------- |
| accountName     | string | 必须   | 用户api账号                                               |                                        |
| wabaId          | string | 必须   | WhatsApp商业账户唯一标识                                      |                                        |
| name            | string | 非必须  | 模板名称(eg\:limited\_time\_offer\_tuscan\_getaway\_2023) | 由小写字母、数字、下划线组合,字段限制为512个字符.            |
| templateId      | string | 非必须  | 模板id                                                  |                                        |
| category        | list   | 非必须  | 模板类型                                                  | 1-MARKETING,2-AUTHENTICATION,3-UTILITY |
| messageLanguage | list   | 非必须  | 模板语言(eg\:en)                                          | 模板支持的语言请参考附录2                          |
| auditStatus     | list   | 非必须  | 模板状态                                                  |                                        |

&#x20;返回参数

| 名称                | 类型        | 是否必须 | 默认值 | 备注                                                                    | 其他信息                                                                                                |
| :---------------- | :-------- | :--- | :-- | :-------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| code              | integer   | 必须   |     |                                                                       | 0：成功其他：失败                                                                                          |
| data              | object    | 非必须  |     |                                                                       |                                                                                                     |
| ├─ total          | integer   | 非必须  |     | 总数                                                                    |                                                                                                     |
| ├─ pages          | integer   | 非必须  |     | 页数                                                                    |                                                                                                     |
| ├─ list           | object    | 非必须  |     |                                                                       |                                                                                                     |
| ├─ id             | integer   | 非必须  |     | 模板主键id                                                                |                                                                                                     |
| ├─ accountName    | string    | 非必须  |     | 用户api账号                                                               |                                                                                                     |
| ├─ wabaId         | string    | 非必须  |     | WhatsApp商业账户唯一标识                                                      |                                                                                                     |
| ├─ name           | string    | 非必须  |     | 模板名称                                                                  | 由小写字母、数字、下划线组合,字段限制为512个字符.                                                                         |
| ├─category        | string    | 非必须  |     | 模板类型                                                                  | 1-MARKETING,2-AUTHENTICATION,3-UTILITY                                                              |
| ├─messageLanguage | string    | 非必须  |     | 模板语言(eg\:en)                                                          | 模板支持的语言请参考附录2                                                                                       |
| ├─headerType      | integer   | 非必须  |     | 是否支持多媒体,默认2                                                           | 1-None,2-Text,3-Image,4-Video,5-Document                                                            |
| ├─caption         | string    | 非必须  |     | 页眉内容                                                                  | 字段限制为字符(多媒体模板时，该字段为空),动参格式{{1}}                                                                     |
| ├─messageBody     | string    | 非必须  |     | 正文内容                                                                  | 字段限制为1024个字符.动参格式{{1}}{{2}},前后按序插入                                                                  |
| ├─footer          | string    | 非必须  |     | 页脚内容                                                                  | 字段限制为60字符(AUTHENTICATION模板时,该字段表示过期时间,非必填,过期时间限制90分钟内)                                              |
| ├─buttons         | object\[] | 非必须  |     | 按钮内容                                                                  | 按钮总数不超过10个                                                                                          |
| ├─ actionType     | integer   | 非必须  |     | 按钮类型                                                                  | 1-quick reply,2-visit website,3-call phone number,4-copy code(category为2时),5-auto fill(category为2时) |
| ├─ label          | string    | 非必须  |     | 按钮文本                                                                  |                                                                                                     |
| ├─ phoneArea      | string    | 非必须  |     | 号码区号(eg:87)                                                           |                                                                                                     |
| ├─ phone          | string    | 非必须  |     | 号码(eg:18877776667)                                                    |                                                                                                     |
| ├─ packageName    | string    | 非必须  |     | 应用包名                                                                  | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ sigNatureHash  | string    | 非必须  |     | 应用哈希散列                                                                | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ autoFill       | string    | 非必须  |     | 自动填充                                                                  | 模板类型为AUTHENTICATION,选填                                                                              |
| ├─ targetUrl      | string    | 非必须  |     | 网址访问(eg："<https://www.chuanglan.com/>{{1}}")                          | 只支持一个动参，格式{{1}}                                                                                     |
| ├─buttonExample   | list      | 非必须  |     | 按钮变量示例(eg："buttonExample": \["<https://www.chuanglan.com/whatsApp>"]) | 网址访问含有变量时必填,变量内容为完整网址                                                                               |
| ├─ headerExample  | object\[] | 非必须  |     | header(caption)变量示例                                                   |                                                                                                     |
| ├─header\_handle  | list      | 非必须  |     | 多媒体模板变量(eg："header\_handle": \["5::axxxx"])                           | 多媒体模板时，填写文件上传返回时句柄内容                                                                                |
| ├─header\_text    | list      | 非必须  |     | 纯文本模板变量(eg："header\_text": \["创蓝云智"])                                 | 文本动参模板变量,动参个数限制1                                                                                    |
| ├─bodyExample     | object\[] | 非必须  |     | body(messageBody)变量示例                                                 |                                                                                                     |
| ├─body\_text      | list      | 非必须  |     | body变量(eg: "body\_text":\[\["variable1","variable3"]])                | 变量示例与变量按序对应，变量{{1}}对应variable1，以此类推                                                                 |
| ├─safetyAdvice    | integer   | 非必须  |     | AUTHENTICATION模板是否添加安全建议(0:不添加 2:添加)                                  | 选填,未添加显示：\*{{1}}\* 是你的验证码。添加后显示：\*{{1}}\* 是你的验证码。为安全起见，请不要分享这组验证码。                                  |

```json
{
    "code": "0",
    "message": "成功",
    "data": {
        "total": 1,
        "pages": 1,
        "list": [
            {
                "updateDate": "2023-10-16 19:09:45",
                "bodyExample": {
                    "body_text": [
                        [
                            "Tom",
                            "II2024089"
                        ]
                    ]
                },
                "buttons": [
                    {
                        "actionType": 3,
                        "phone": "15121041046",
                        "phoneArea": "86",
                        "label": "Call Us"
                    },
                    {
                        "actionType": 2,
                        "buttonExample": [
                            "\"https://www.chuanglan.com/document\""
                        ],
                        "label": "Websit",
                        "targetUrl": "https://www.chuanglan.com/{{1}}"
                    }
                ],
                "messageBody": "Dear customer {{1}}，your SMS API account {{2}} is avaliable, please rember the secret of this account. The secret is sent via email.",
                "accountName": "accountName",
                "footer": "Have a nice sms experience.",
                "messageLanguage": "zh_CN",
                "caption": "Transland API Account {{1}} Opened",
                "updateUser": "",
                "source": 1,
                "delFlag": 0,
                "templateId": "FCFD7669C142497684F090203F88AE98",
                "customerName": "CXQ测试20230426",
                "sid": "634562468792622",
                "headerExample": {
                    "header_text": [
                        "II2024089"
                    ]
                },
                "wabaId": "wabaId",
                "name": "transland_marketing_promotion_01",
                "auditStatus": 2,
                "headerType": 2,
                "createUser": "",
                "id": 194,
                "category": 1,
                "createDate": "2023-10-16 19:09:45",
                "safetyAdvice": 0
            }
        ]
    }
}
```

## 1.5 模板删除

**Path：** /intwa-api/whatsapp/template/delete

**Content-Type:** application/json

**Method：** POST

**接口描述：**

请求参数

| 名称          | 类型      | 是否必须 | 备注               | 其他信息 |
| :---------- | :------ | :--- | :--------------- | :--- |
| accountName | string  | 必须   | 用户api账号          |      |
| wabaId      | string  | 必须   | WhatsApp商业账户唯一标识 |      |
| id          | integer | 必须   | 模板主键id           |      |

&#x20;返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息       |
| :------ | :------ | :--- | :-- | :- | :--------- |
| code    | integer | 必须   |     |    | 0：成功<br/>其他：失败 |
| data    | object  | 必须   |     |    |            |
| message | string  | 必须   |     |    |            |

