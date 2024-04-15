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
| phoneNumber    | string  | 必须   | 手机号码         |      |

&#x20;返回参数

| 名称      | 类型      | 是否必须 | 默认值 | 备注 | 其他信息       |
| :------ | :------ | :--- | :-- | :- | :--------- |
| code    | integer | 必须   |     |    | 0：成功<br/>其他：失败 |
| data    | object  | 必须   |     |    |            |
| message | string  | 必须   |     |    |            |
