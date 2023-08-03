# WhatsApp Webhook
## Event字段
| 字段名称  | 类型   | 备注       | 示例                                 |
| --------- | ------ | ---------- | ------------------------------------ |
| id        | String | Event Id   | e5cb1bc6-ad90-419d-8f83-aa394c0b7cc7 |
| type      | String | Event 类型 | whatsapp_message_status_updated      |
| eventTime | String | Event时间  | 2023-02-22T12:00:00.000Z             |
| body      | JSON   | 消息体     |                                      |

### 状态报告消息体：body

| 字段名称     | 类型   | 备注                              | 示例                                   |
| ------------ | ------ | --------------------------------- | -------------------------------------- |
| id           | String | Message Id                        | 356139161272397824                     |
| accountName  | String | API账号名称                       | IW123456                               |
| wabaId       | String | Waba Id                           | 11231231212331                         |
| status       | String | 消息状态：delivered、read、failed | delivered                              |
| wamid        | String | WhatsApp消息Id                    | wamid.HBgNODYxNjY4NTE3NzYxMhUCABEYEjQ0 |
| conversation | JSON   | 会话信息，status是delivered时有值 |                                        |
| errorData    | JSON   | 错误消息，status是failed时有值    |                                        |



#### conversation格式

| 字段名称     | 类型   | 备注         | 示例                             |
| ------------ | ------ | ------------ | -------------------------------- |
| id           | String | 会话Id       | 00e5a7e14a588d96bd2343d105d03ec5 |
| initiateType | String | 会话发起类型 | business_initiated               |
| expireAt     | String | 会话过期时间 | 2023-02-23T12:00:00.000Z         |

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

## Event Type Code

| Event Type |  时间名称  | 
| :----: | :----: |
|  whatsapp_message_status_updated  | 消息状态报告 |
