# WhatsApp Webhook
Waba有消息更新时，会通过Webhook将更新事件推送给客户。包括消息状态更新、用户回复消息、Waba账号审核状态更新、WhatsApp模板状态更新、发送号码显示名称变更等。
## Event字段
| 字段名称  | 类型   | 备注       | 示例                                 |
| --------- | ------ | ---------- | ------------------------------------ |
| id        | String | Event Id   | e5cb1bc6-ad90-419d-8f83-aa394c0b7cc7 |
| type      | String | [Event 类型](#event-type-code) | whatsapp_message_status_updated      |
| eventTime | String | Event时间  | 2023-02-22T12:00:00.000Z             |
| body      | JSON   | 消息体     |                                      |

### 状态报告消息体：body

| 字段名称     | 类型   | 备注                              | 示例                                   |
| ------------ | ------ | --------------------------------- | -------------------------------------- |
| id           | String | Message Id                        | 356139161272397824                     |
| accountName  | String | API账号名称                       | IW123456                               |
| wabaId       | String | Waba Id                           | 11231231212331                         |
| status       | String | 消息状态：delivered、read、failed、SMECL:FAILED | delivered                              |
| wamid        | String | WhatsApp消息Id                    | wamid.HBgNODYxNjY4NTE3NzYxMhUCABEYEjQ0 |
| conversation | JSON   | 会话信息，status是delivered时有值 |                                        |
| errorData    | JSON   | 错误消息，status是failed时有值    |                                        |



#### conversation格式

| 字段名称     | 类型   | 备注         | 示例                                                         |
| ------------ | ------ | ------------ | ------------------------------------------------------------ |
| id           | String | 会话Id       | 00e5a7e14a588d96bd2343d105d03ec5                             |
| initiateType | String | 会话发起类型 | business_initiated：商家发起会话<br/>referral_conversion：免费入口发起会话，这类会话由用户发起<br/>customer_initiated：用户发起会话 |
| expireAt     | String | 会话过期时间 | 2023-02-23T12:00:00.000Z                                     |

#### errorData格式

| 字段名称     | 类型   | 备注     | 示例   |
| ------------ | ------ | -------- | ------ |
| errorCode    | String | Meta错误码   | 131014 |
| errorMessage | String | Meta错误描述 |        |


#### 示例
##### 状态报告：delivered
```json
{
    "id": "e5cb1bc6-ad90-419d-8f83-aa394c0b7cc7",
    "type": "whatsapp_message_status_updated",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "356139161272397824",
        "accountName": "IW123456",
        "wabaId": "11231231212331",
        "wamid": "wamid.BgNODYxN...", 
        "status": "delivered",
        "conversation": {
            "id": "00e5a7e14a588d96bd2343d105d03ec5",
            "initiateType": "business_initiated",
            "expireAt": "2023-02-23T12:00:00.000Z"
        },
        "currency": "USD"
    }
}
```

##### 状态报告：read
```json
{
    "id": "cd0a316c-a781-4589-9f5f-5502ccf1f60f",
    "type": "whatsapp_message_status_updated",
    "eventTime": "2023-05-26T02:18:44.115Z",
    "body": {
        "accountName": "15902677617",
        "id": "356139161272397824",
        "status": "read",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNODYxNjY4NTE3NzYxMhUCABEYEjQ0RTYyQTM5QzAyMkU0QkNERgA="
    }
}
```

##### 状态报告：failed
```json
{
    "id": "e5cb1bc6-ad90-419d-8f83-aa394c0b7cc7",
    "type": "whatsapp_message_status_updated",
    "eventTime": "2023-05-25T10:31:08.167Z",
    "body": {
        "accountName": "15902677617",
        "errorData": {
            "errorCode": "131014",
            "errorMessage": "Request for url https://URL.jpg failed with error: 404 (Not Found)"
        },
        "id": "356139161272397824",
        "status": "failed",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNODYxNjY4NTE3NzYxMhUCABEYEjQ0RTYyQTM5QzAyMkU0QkNERgA="
    }
}
```





### 上行消息 消息体结构

| **Name**        | Type   | **Description**                                              |
| --------------- | ------ | ------------------------------------------------------------ |
| id              | String | 消息Id                                                       |
| accountName     | String | API账号名称                                                  |
| wabaId          | String | Waba Id                                                      |
| wamid           | String | Meta WhatApp消息Id                                           |
| from            | String | 用户手机号                                                   |
| to              | String | 商户手机号                                                   |
| sendTime        | String | 发送时间，格式：2023-02-22T12:00:00.000Z                     |
| customerProfile | JSON   | 用户信息                                                     |
| type            | String | 消息类型；<br/>text：文本消息<br/>reaction：表情符号消息 <br/>image:图片消息<br/>audio:音频消息<br/>video:视频消息<br/>sticker:贴纸消息<br/>unknown：未知消息<br/>location：位置消息<br/>contacts：联系人消息<br/>button：模板按钮回复消息<br/>interactive：交互式消息/交互按钮回复消息<br/>order：订单消息<br/>system：用户更改号码通知<br/> |
| text            | JSON   | type=text时有值                                              |
| reaction        | JSON   | type=reaction时有值                                          |
| image           | JSON   | type=image时有值                                             |
| audio           | JSON   | type=audio时有值                                             |
| video           | JSON   | type=video时有值                                             |
| errors          | JSON   | type=unknown时有值                                           |
| location        | JSON   | type=location时有值                                          |
| contacts        | JSON   | type=contacts时有值                                          |
| button          | JSON   | type=button时有值                                            |
| system          | JSON   | type=system时有值                                            |
| context         | JSON   | 上下文消息，引用消息回复时有值                               |



### 上行消息Examples:

#### 文本消息

从客户处收到的文本消息,`text.body`表示回复的内容

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.BgNODYxN...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "text",
        "text": {
            "body": "OK"
        }
    }
}
```



#### 表情符号消息 

从客户处收到的表情符号消息：

`sendTime`表示客户发送表情符号的时间

`reaction.messageId`表示引用消息的wamid(原始消息id)

`reaction.emoji`表示回复的表情符号

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "reaction",
        "reaction": {
            "messageId": "wamid.HBgNODY...",
            "emoji": "EMOJI"
        }
    }
}
```



#### 媒体消息
媒体消息推送内容中，有`link`字段，是媒体文件的下载链接。参照 [根据返回的link查看媒体文件](#根据返回的link查看媒体文件)

##### 图像消息

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "image",
        "image": {
            "caption": "CAPTION",
            "mimeType": "image/jpeg",
            "sha256": "IMAGE_HASH",
            "link": "http://xxxxxxxxxx"
        }
    }
}
```

##### 贴图消息

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "sticker",
        "sticker": {
            "mimeType": "image/webp",
            "sha256": "HASH",
            "link": "http://xxxxxxxxxx"
        }
    }
}
```

##### 视频消息

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "video",
        "video": {
            "mimeType": "video/mp4",
            "sha256": "G4fboj5cKcAbCdefzhcAcRdnXJqFJAyTSlNmJANhu4M=",
            "link": "http://xxxxxxxxxx"
        }
    }
}
```

##### 音频消息

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "audio",
        "audio": {
            "mimeType": "audio/ogg; codecs=opus",
            "sha256": "ffRSAbcDeff3mJ2hBhpzuFY7pYEugTfglD+zx4Qv8X4=",
            "link": "http://xxxxxxxxxx"
        }
    }
}
```

##### 文档消息

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "document",
        "document": {
            "caption": "pdf caption",
            "filename": "filename.pdf",
            "mimeType": "application/pdf",
            "sha256": "TJGGMF5tdw3XApVHbABCdeffI7w4OW7GqYEN736PW0s=",
            "link": "http://xxxxxxxxxx"
        }
    }
}
```

#### 位置消息

从客户处收到的位置消息：

`location.latitude`表示纬度

`location.longitude`表示经度

`location.name`表示位置名称

`location.address`表示地址

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "location",
        "location": {
            "latitude": 39.90539,
            "longitude": 116.39134,
            "name": "LOCATION_NAME",
            "address": "LOCATION_ADDRESS",
        }
    }
}
```

##### 联系人消息

```JSON
{
    "body": {
        "accountName": "IW31****5",
        "contact": [
            {
                "addresses": [
                    {
                        "city": "CONTACT_CITY1",
                        "country": "CONTACT_COUNTRY1",
                        "countryCode": "CONTACT_COUNTRY_CODE1",
                        "state": "CONTACT_STATE1",
                        "street": "CONTACT_STREET1",
                        "type": "HOME or WORK1",
                        "zip": "CONTACT_ZIP1"
                    }
                ],
                "birthday": "CONTACT_BIRTHDAY1",
                "emails": [
                    {
                        "email": "CONTACT_EMAIL1",
                        "type": "WORK or HOME1"
                    }
                ],
                "name": {
                    "firstName": "CONTACT_FIRST_NAME1",
                    "formattedName": "CONTACT_FORMATTED_NAME1",
                    "lastName": "CONTACT_LAST_NAME1",
                    "middleName": "CONTACT_MIDDLE_NAME1",
                    "prefix": "CONTACT_PREFIX1",
                    "suffix": "CONTACT_SUFFIX1"
                },
                "org": {
                    "company": "CONTACT_ORG_COMPANY1",
                    "department": "CONTACT_ORG_DEPARTMENT1",
                    "title": "CONTACT_ORG_TITLE1"
                },
                "phones": [
                    {
                        "phone": "CONTACT_PHONE1",
                        "type": "HOME or WORK>1",
                        "wa_id": "CONTACT_WA_ID1"
                    }
                ],
                "urls": [
                    {
                        "type": "HOME or WORK1",
                        "url": "CONTACT_URL1"
                    }
                ]
            }
        ],
        "customerProfile": {
            "name": "Jack"
        },
        "from": "86183****2197",
        "id": "a1301cb6d0094b2fae36546c22e09044",
        "sendTime": "2024-03-07T10:46:24.000Z",
        "to": "62811****6819",
        "type": "contacts",
        "wabaId": "1**********9",
        "wamid": "wamid.HBgNODYxODM1NTA5MjE5NxUCABIYIDg3RDVFMzQyRjIwQkM5NDQyMDI5OTRERERGNUYx*****=="
    },
    "eventTime": "2024-03-08T09:42:06.363Z",
    "id": "e2bafad5-0aa9-4465-9ed1-c5c01741c5e1",
    "type": "whatsapp_mo_message_received"
}
```



#### 模板按钮回复消息

客户点击[互动消息模板](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/messages#interactive-message-templates)中的快速回复按钮时，系统会发送响应。以下是回调格式示例。

- `context.from`是发送交互式消息的 WhatsApp ID（不带“+”前缀的电话号码）。
- `context.id`是WhatsApp平台上的原始消息ID。

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "button",
        "button": {
            "text": "No",
            "payload": "No-Button-Payload"
        },
        "context": {
            "from": "PHONE_NUMBER",
            "id": "wamid.ID"
        }
    }
}
```





#### 未知消息

您可能会收到未知的消息回调通知。例如，客户可能会给您发送不受支持的消息，如限时消息（在这种情况下，我们会通知客户消息类型不受支持）。

以下是从客户处收到的不受支持的消息示例:

`errors.code`表示错误码

`errors.details`表示错误详情

`errors.title`表示错误名称

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "unknown",
        "errors": [
            {
                "code": 131051,
                "details": "Message type is not currently supported",
                "title": "Unsupported message type"
            }
        ]
    }
}
```

#### 交互式消息

当用户点击您发送的清单消息中的某一项时，您会收到以下 Webhooks 通知：

- `context.from`是发送交互式消息的 WhatsApp ID（不带“+”前缀的电话号码）。
- `context.id`是WhatsApp平台上的原始消息ID。

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "interactive",
        "interactive": {
            "listReply": {
                "id": "list_reply_id",
                "title": "list_reply_title",
                "description": "list_reply_description"
            },
            "type": "listReply"
        },
        "context": {
            "from": "PHONE_NUMBER",
            "id": "wamid.ID"
        }
    }
}
```

#### 交互按钮回复消息

当用户点击您发送的回复按钮时，您会收到以下 Webhooks 通知：

- `context.from`是发送交互式消息的 WhatsApp ID（不带“+”前缀的电话号码）。
- `context.id`是WhatsApp平台上的原始消息ID。

```JSON
{
    "id": "b5e984b3-21b2-42a9-b08b-72e5a4096ac6",
    "type": "whatsapp_mo_message_received",
    "eventTime": "2023-02-22T12:00:00.000Z",
    "body": {
        "id": "63f5d602367ea403f8175a6c",
        "accountName": "IW123456",
        "wabaId": "WHATSAPP_BUSINESS_ACCOUNT_ID",
        "wamid": "wamid.HBgNOD...",
        "from": "PHONE-NUMBER",
        "customerProfile": {
            "name": "Jack"
        },
        "to": "BUSINESS-PHONE-NUMBER",
        "sendTime": "2023-02-22T12:00:00.000Z",
        "type": "interactive",
        "interactive": {
            "buttonReply": {
                "id": "unique-button-identifier-here",
                "title": "button-text"
            },
            "type": "buttonReply"
        },
         "context": {
            "from": "PHONE_NUMBER",
            "id": "wamid.ID"
        }
    }
}
```

### 模板回调消息体：body

| 字段名称         | 类型   | 备注                    | 示例                                                         |
| ---------------- | ------ | ----------------------- | ------------------------------------------------------------ |
| accountName      | String | 账号                    |                                                              |
| category         | String | 模板类型                | 模板类型<br/>MARKETING:营销<br/>AUTHENTICATION:验证码<br/>UTILITY:通知|
| reason           | String | 理由                    |                                                              |
| templateId       | String | meta平台模板id(对应sid) |                                                              |
| templateLanguage | String | 模板语言                |                                                              |
| templateName     | String | 模板名称                |                                                              |
| templateStatus   | String | 模板状态                | PENDING:审核中<br/>APPROVED:审核通过<br/>REJECTED:驳回<br/>PAUSED:暂停<br/>DISABLED:禁用 |
| wabaId           | String | wabaId                  |                                                              |

#### 示例

##### 回调示例：APPROVED

```JSON
{
    "id": "fd201190-50dc-4151-baec-1d16c8a704e1",
    "type": "whatsapp_template_status_updated",
    "eventTime": "2023-10-16T13:04:57.644Z",
    "body": {
        "accountName": "IW2267527",
        "category": "AUTHENTICATION",
        "reason": "NONE",
        "templateId": "998961841525295",
        "templateLanguage": "en_US",
        "templateName": "transland_common_otp",
        "templateStatus": "APPROVED",
        "wabaId": "110129512080569"
    }
}
```

### 根据返回的link查看媒体文件
1. ### Request URL
GET http://waapi.tig253.com/intwa-api/whatsapp/media/download/{媒体id}

#### Request Header

| 参数名 |  类型  | 是否必填 |          备注          |               示例               |
| :----: | :----: | :------: | :--------------------: | :------------------------------: |
|  sign  | String |    Y     | 签名，参考接口加密规则 | 70cc57ad4ea2eea0960c9ee40234567c |
| nonce  | String |    Y     |         随机数         |              123456              |

1. ### Request Params

|   参数名    |  类型  | 是否必填 |  备注   | 示例 |
| :---------: | :----: | :------: | :-----: | :--: |
| accountName | string |   必须   | API账号 |      |
|   wabaId    | string |   必须   | wabaId  |      |

#### 请求示例
```
请求头加sign和nonce参数
http://waapi.tig253.com/intwa-api/whatsapp/media/download/123456789?accountName=IW123456&wabaId=10023456789
```
#### response

文件

#### 接口签名规则
参照发送接口加签规则


## Event Type Code

| Event Type |  事件名称  | 状态 |
| :----: | :----: | ------ |
|  whatsapp_message_status_updated  | 消息状态报告 | 可用 |
| whatsapp_mo_message_received | 上行消息 | 可用 |
| whatsapp_template_status_updated | 模板状态更新 | 可用 |
| whatsapp_account_review_updated | 账号审核状态更新 | 更新中 |
| whatsapp_phone_number_name_update | 发送号码名称更新 | 更新中 |
