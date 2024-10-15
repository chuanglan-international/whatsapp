# WhatsApp账号相关
##
## 1.1 账号信息绑定

**Path：** /intwa-api/whatsapp/account/bind

**Content-Type:** application/json

**Method：** POST

**接口描述：**

请求参数

| 名称          | 类型      | 是否必须 | 备注               | 其他信息 |
| :---------- | :------ | :--- | :--------------- | :--- |
| accountName    | string  | 必须   | 用户api账号          |      |
| wabaId         | string  | 必须   | WhatsApp商业账户唯一标识 |      |
| phoneNumberId  | string  | 必须   | 手机号id          |      |
| phoneNumber    | string  | 非必须   | 手机号码         |      |

&#x20;返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息       |
| :------ | :------ | :--- | :-- | :- | :--------- |
| code    | integer | 必须   |     |    | 0：成功<br/>其他：失败 |
| data    | object  | 必须   |     |    |            |
| message | string  | 必须   |     |    |            |

*   请求参数示例

    ```json
     {
        "accountName": "IW****",
        "wabaId":"27899******57661",
        "phoneNumberId":"2673*****93634",
        "phoneNumber":"86133****5155"
    }
    ```




##
## 1.2 手机信息查询

**Path：** /intwa-api/whatsapp/account/phoneInfo

**Content-Type:** application/json

**Method：** POST

**接口描述：**

请求参数

| 名称          | 类型      | 是否必须 | 备注               | 其他信息 |
| :---------- | :------ | :--- | :--------------- | :--- |
| accountName    | string  | 必须   | 用户api账号          |      |
| wabaId         | string  | 必须   | WhatsApp商业账户唯一标识 |      |


&#x20;返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息       |
| :------ | :------ | :--- | :-- | :- | :--------- |
| code    | integer | 必须   |     |    | 0：成功<br/>其他：失败 |
| message | string  | 必须   |     |    |            |
| data    | object  | 必须   |     |    |            |
| ├─ id    | string  | 必须   |     |    |            |
| ├─ displayPhoneNumber    | string  | 必须   |     |    |            |
| ├─ verifiedName    | string  | 必须   |     |    |            |
| ├─ status    | string  | 必须   |     |    |            |
| ├─ qualityRating    | string  | 必须   |     |    |            |
| ├─ messageLimit    | string  | 必须   |     |    |            |


*   请求参数示例

    ```json
     {
        "accountName": "IW****",
        "wabaId":"27899******57661"
    }
    ```

*   请求响应示例

    ```json
    {
    "code": "0",
    "message": "Success",
    "data": [
        {
            "id": "3142****760025",
            "displayPhoneNumber": "+62 852-****-0016",
            "verifiedName": "name",
            "status": "CONNECTED",
            "qualityRating": "GREEN",
            "messageLimit": "TIER_250"
        }
    ]}


##
## 1.3 waba信息查询

**Path：** /intwa-api/whatsapp/account/wabaInfo

**Content-Type:** application/json

**Method：** POST

**接口描述：**

请求参数

| 名称          | 类型      | 是否必须 | 备注               | 其他信息 |
| :---------- | :------ | :--- | :--------------- | :--- |
| accountName    | string  | 必须   | 用户api账号          |      |


&#x20;返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息       |
| :------ | :------ | :--- | :-- | :- | :--------- |
| code    | integer | 必须   |     |    | 0：成功<br/>其他：失败 |
| message | string  | 必须   |     |    |            |
| data    | object  | 必须   |     |    |            |
| ├─ wabaId    | string  | 必须   |     |    |            |
| ├─ wabaName    | string  | 必须   |     |    |            |
| ├─ wabaStatus    | string  | 必须   |     |    |            |



*   请求参数示例

    ```json
     {
        "accountName": "IW****"
     }
    ```

*   请求响应示例

    ```json
    {
        "code": "0",
        "message": "Success",
        "data": [
            {
                "wabaId": "***123****",
                "wabaName": "PT TIG TECH ",
                "wabaStatus": "APPROVED"
            },
            {
                "wabaId": "*****456****",
                "wabaName": "PT TIG CODE ",
                "wabaStatus": "APPROVED"
            }
        ]
    }    
