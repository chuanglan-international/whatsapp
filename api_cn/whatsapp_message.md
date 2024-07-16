# WhatsApp消息发送接口

# Request URL

Post https://waapi.tig253.com/intwa-api/whatsapp/msg

## Request Header

| 参数名 |  类型  | 是否必填 |          备注          |               示例               |
| :----: | :----: | :------: | :--------------------: | :------------------------------: |
|  sign  | String |    Y     | 参数签名，详见签名规则 | 1424e0e4c9b8c740ea0231b0800bbf92 |
| nonce  | String |    Y     |     随机数或时间戳     |          1679903484890           |

## Request Body

|     参数名      |      类型      | 是否必填 |                             备注                             |                             示例                             |
| :-------------: | :------------: | :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|     account     |     String     |    Y     |                         API账号/apiKey                         |                          IW253654/wbUh8ds50jxACBA                           |
|     wabaId      |     String     |    Y     |                           waba账号                           |                       110129512080556                        |
|   messageType   |     String     |    Y     | 消息内容类型template:模板text:文本image:图片video:视频audio:音频location:定位document:文档sticker:贴图*contacts：联系人**interactive：互动消息* |                            image                             |
|       uid       |     String     |    N     |                   客户批次号,不超过100字符                   |                      202303306668899999                      |
|   sendNumber    |     String     |    Y     |                发送号码，格式：国家码+手机号                 |                        8618712345678                         |
| recipientNumber |     String     |    Y     | 接收号码，格式：国家码+手机号多个号码使用逗号分隔，最多2000个号码 |                        6282123456789                         |
|      body       |     String     |    N     |                消息体，messageType=text时必填                |      This is the first WhatsApp message from Chuanglan.      |
|    language     |     String     |    N     | 消息语言，详见 附：Language Code ListmessageType=template时必填 |                            en_US                             |
|  templateName   |     String     |    N     | 模板名称。允许小写字母、数字、字符和下划线，必须唯一。messageType=template时必填 |                         notify_tmplt                         |
|     header      |   HeaderDto    |    N     |               模板的消息头信息。详见 HeaderDto               | {        "type":"image",  "link":"https://www.pianshen.com/thumbs/886/535cfdc357b166dc020c70d0533d85f6.JPEG" } |
|   bodyParams    |    String[]    |    N     |                    模板正文中有变量时必填                    |           ["TIG", "123456"]按照模板中变量顺序传值            |
|  buttonParams   | ParameterDto[] |    N     |        模板按钮类型是动态链接时必填详见 ParameterDto         |            [{"type":"text","value":"yHkYRww90"}]             |
|      media      |    MediaDto    |    N     | messageType=image/video/audio/document/sticker时必填详见 MediaDto | {    "link": "http://oss-ycloud-publicread.oss-ap-southeast-1.aliyuncs.com/online/BASE-FILE/2023/03/24/cd56097f-c888-4be5-961d-584d0fa150f3.jpg",    "caption":"this is a image message"} |
|    location     |  LocationDto   |    N     |          messageType=location时必填详见 LocationDto          | {        "latitude": "-5.15161",        "longitude":"119.41629",        "name":"Pizza Hut",        "address":"xxx road No.123" } |
|    contacts     |  ContactDto[]  |    N     |         messageType=*contacts*时必填详见 ContactDto          | [    {        "addresses": [            {                "city": "city name",                "country": "country name",                "country_code": "code",                "state": "Contact's State",                "street": "Contact's Street",                "type": "Contact's Address Type",                "zip": "Contact's Zip Code"            }        ],        "birthday": "birthday",        "emails": [            {                "email": "email",                "type": "HOME"            },            {                "email": "email",                "type": "WORK"            }        ],        "name": {            "first_name": "first name value",            "formatted_name": "formatted name value",            "last_name": "last name value",            "suffix": "suffix value"        },        "org": {            "company": "company name",            "department": "dep name",            "title": "title"        },        "phones": [            {                "phone": "Phone number",                "wa-id": "WA-ID value",                "type": "MAIN"            },            {                "phone": "Phone number",                "type": "HOME"            },            {                "phone": "Phone number",                "type": "WORK"            }        ],        "urls": [{            "url": "some url",            "type": "WORK"        }]    } ] |
|   interactive   | InteractiveDto |    N     |      messageType=*interactive*时必填详见 InteractiveDto      | {        "type": "list",        "header": {            "type": "text",            "text": "5G Message header"        },        "body": {            "text": "meta Message Text "        },        "footer": {            "text": "meta Message Text Footer"        },        "action": {            "button": "测试按钮",            "sections": [                {                    "title": "meta Message Title1",                    "rows": [                        {                            "id": "20230524001",                            "title": "<SECTION_1_ROW_1_TITLE>",                            "description": "<SECTION_1_ROW_1_DESC>"                        },                        {                            "id": "20230524002",                            "title": "<SECTION_1_ROW_2_TITLE>",                            "description": "<SECTION_1_ROW_2_DESC>"                        }                    ]                },                {                    "title": "meta Message Title2",                    "rows": [                        {                            "id": "20230524003",                            "title": "<SECTION_2_ROW_1_TITLE>",                            "description": "<SECTION_2_ROW_1_DESC>"                        },                        {                            "id": "20230524004",                            "title": "<SECTION_2_ROW_2_TITLE>",                            "description": "<SECTION_2_ROW_2_DESC>"                        }                    ]                }            ]        }    } |

### HeaderDto

|   参数名   |  类型  | 是否必填 |                备注                 |                             示例                             |
| :--------: | :----: | :------: | :---------------------------------: | :----------------------------------------------------------: |
|    type    | String |    Y     | 媒体类型：text/image/video/document |                            image                             |
|    link    | String |    N     |     媒体链接type不是text时必填      | https://p0.itc.cn/images01/20230201/1444f46dd92a4f6c8a626b585eb52751.png |
|  caption   | String |    N     |       标题type不是text时有效        |                   Caption for the Picture                    |
| paramValue | String |    N     | 变量type=text时有效，仅支持一个变量 |                             tig                              |

### ParameterDto

| 参数名 |  类型  | 是否必填 |                   备注                   |  示例  |
| :----: | :----: | :------: | :--------------------------------------: | :----: |
|  type  | String |    Y     |             类型，一般为text             |  text  |
| value  | String |    N     | 变量值如果是按钮上的文字，不超过20个字符 | uYgEYm |

### MediaDto

|  参数名  |  类型  | 是否必填 |            备注            |                             示例                             |
| :------: | :----: | :------: | :------------------------: | :----------------------------------------------------------: |
|   link   | String |    Y     |            必填            | https://p0.itc.cn/images01/20230201/1444f46dd92a4f6c8a626b585eb52751.png |
| caption  | String |    N     |            标题            |                   Caption for the Picture                    |
| filename | String |    N     | messageType=document时必填 |                       introduction.pdf                       |

1. ### LocationDto

|  参数名   |  类型  | 是否必填 |   备注   |       示例       |
| :-------: | :----: | :------: | :------: | :--------------: |
| latitude  | String |    Y     |   纬度   |     31.23593     |
| longitude | String |    Y     |   经度   |    121.48054     |
|   name    | String |    Y     | 位置名称 |     Pizz Hut     |
|  address  | String |    Y     |   地址   | xxxx Road No.666 |

### ContactDto

|  参数名   |      类型      | 是否必填 |              备注              |                             示例                             |
| :-------: | :------------: | :------: | :----------------------------: | :----------------------------------------------------------: |
| addresses |   Address[]    |    N     |     地址信息详见：Address      | [            {                "city": "city name",                "country": "country name",                "countryCode": "code",                "state": "Contact's State",                "street": "Contact's Street",                "type": "Contact's Address Type",                "zip": "Contact's Zip Code"            }        ] |
| birthday  |     String     |    N     |              生日              |                          2005-05-23                          |
|  emails   | ContactEmail[] |    N     |   邮箱信息详见：ContactEmail   | [  {                "email": "email",                "type": "HOME"        },            {                "email": "email",                "type": "WORK"      }] |
|   name    |  ContactName   |    Y     |       详见：ContactName        | {            "firstName": "first name value",            "formattedName": "formatted name value",            "lastName": "last name value",            "suffix": "suffix value"        } |
|    org    |   ContactOrg   |    N     | 联系人组织信息详见：ContactOrg | {            "company": "company name",            "department": "dep name",            "title": "title"        } |
|  phones   |    Phone[]     |    N     |     电话信息：详见：Phone      | [            {                "phone": "Phone number",                "waId": "WA-ID value",                "type": "MAIN"            },            {                "phone": "Phone number",                "type": "HOME"            },            {                "phone": "Phone number",                "type": "WORK"            }        ] |
|   urls    |  ContactUrl[]  |    N     |     url信息详见:ContactUrl     | [{            "url": "some url",            "type": "WORK"        }] |

#### Address

|   参数名    |  类型  | 是否必填 |   备注   |      示例       |
| :---------: | :----: | :------: | :------: | :-------------: |
|   street    | String |    N     | 街道信息 |   XX街道XX号    |
|    city     | String |    N     |   城市   |      北京       |
|    state    | String |    N     |  州缩写  | Contact's State |
|     zip     | String |    N     | 邮政编码 |     000001      |
|   country   | String |    N     |   国家   |                 |
| countryCode | String |    N     |  国家码  |                 |
|    type     | String |    N     | 地址类型 |    HOME/WORK    |

#### ContactEmail

| 参数名 |  类型  | 是否必填 |   备注   |      示例       |
| :----: | :----: | :------: | :------: | :-------------: |
| email  | String |    N     | 邮箱地址 | xxxxx@gmail.com |
|  type  | String |    N     |   类型   |    HOME/WORK    |

#### ContactName

|    参数名     |  类型  | 是否必填 |    备注     |     示例     |
| :-----------: | :----: | :------: | :---------: | :----------: |
| formattedName | String |    Y     |    全名     | Stephen Chow |
|   firstName   | String |    N     | First name  |              |
|   lastName    | String |    N     |  Last name  |              |
|  middleName   | String |    N     | Middle name |              |
|    suffix     | String |    N     | Name suffix |              |
|    prefix     | String |    N     | Name prefix |              |

#### ContactOrg

|   参数名   |  类型  | 是否必填 |      备注      | 示例 |
| :--------: | :----: | :------: | :------------: | :--: |
|  company   | String |    N     | 联系人公司名称 |      |
| department | String |    Y     | 联系人部门名称 |      |
|   title    | String |    Y     | 联系人业务名称 |      |

#### Phone

| 参数名 |  类型  | 是否必填 |    备注     |   示例    |
| :----: | :----: | :------: | :---------: | :-------: |
| phone  | String |    Y     |    phone    |           |
|  type  | String |    Y     |    类型     | HOME/WORK |
|  waID  | String |    Y     | WhatsApp ID |           |

#### ContactUrl

| 参数名 |  类型  | 是否必填 | 备注 |            示例            |
| :----: | :----: | :------: | :--: | :------------------------: |
|  url   | String |    Y     | URL  | https://www.chuanglan.com/ |
|  type  | String |    Y     | 类型 |         HOME/WORK          |

### InteractiveDto

| 参数名 |       类型        | 是否必填 |                             备注                             |                             示例                             |
| :----: | :---------------: | :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  type  |      String       |    Y     |        互动消息类型:list/button/product/product_list         |                             list                             |
| header | InteractiveHeader |    N     | 互动消息请求头：product_list 类型时候必要、 product类型不能设置header 其他类型可选详见：InteractiveHeader | {      "type": "text",      "text": "your-header-content-here"    } |
|  body  |       Body        |    Y     |       类型为product时可选，其他消息类型必需 详见：Body       |               {"text":"you message body text"}               |
| footer |      Footer       |    N     |                       页脚详见：Footer                       |        {      "text": "your-footer-content-here"    }        |
| action |      Action       |    Y     |                             操作                             | {      "button": "cta-button-content-here",      "sections":[        {          "title":"your-section-title-content-here",          "rows": [            {              "id":"unique-row-identifier-here",              "title": "row-title-content-here",              "description": "row-description-content-here",                       }          ]        },        {          "title":"your-section-title-content-here",          "rows": [            {              "id":"unique-row-identifier-here",              "title": "row-title-content-here",              "description": "row-description-content-here",                       }          ]        }      ]    } |

#### InteractiveHeader

|  参数名  |  类型  | 是否必填 |                  备注                  |                  示例                  |
| :------: | :----: | :------: | :------------------------------------: | :------------------------------------: |
|   type   | String |    Y     |    类型：text/image/document/video     |                  text                  |
|   text   | String |    N     |        文本内容type=text时必填         |               your text                |
| document | Media  |    N     | 文档：type=document时必填详见下面Media | {        "link": "document url"      } |
|  video   | Media  |    N     |   视频:type=video时必填详见下面Media   |  {        "link": "video url"      }   |
|  image   | Media  |    N     |   图片:type=image时必填详见下面Media   |  {        "link": "image url"      }   |

##### Media

| 参数名 |  类型  | 是否必填 |         备注          |       示例        |
| :----: | :----: | :------: | :-------------------: | :---------------: |
|  link  | String |    Y     | 文件、视频、图片的url | https://xxxxx.jpg |

#### Body

| 参数名 |  类型  | 是否必填 |   备注   |      示例      |
| :----: | :----: | :------: | :------: | :------------: |
|  text  | String |    Y     | 文本内容 | You  body text |

#### Footer

| 参数名 |  类型  | 是否必填 |   备注   |      示例       |
| :----: | :----: | :------: | :------: | :-------------: |
|  text  | String |    Y     | 文本内容 | You footer text |

#### Action

|      参数名       |   类型    | 是否必填 |                             备注                             |                             示例                             |
| :---------------: | :-------: | :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|     catalogId     |  String   |    Y     | type=product/product_type时必填：链接到您的WhatsApp商业帐户的Facebook目录的唯一标识符。这个ID可以通过Commerce Manager检索 |                                                              |
| productRetailerId |  String   |    Y     | type=product/product_type时必填： 产品在目录中的唯一标识符。单产品和多产品消息最多100个字符。 |                    product-SKU-in-catalog                    |
|      buttons      | Button[]  |    Y     |                   按钮对象： 详见：Button                    | [        {          "type": "reply",          "reply": {            "id": "unique-postback-id",            "title": "First Button’s Title"           }        },        {          "type": "reply",          "reply": {            "id": "unique-postback-id",            "title": "Second Button’s Title"           }        }      ] |
|      button       |  String   |    Y     |          按钮内容： type=list时必填，不能为空字符串          |                        Button contont                        |
|     sections      | Section[] |          | 清单消息(type=list)和多产品消息（type=product_list）必填。详见：Section | [             {             "title": "section-title",                          "product_items": [                  { "productRetailerId": "product-SKU-in-catalog" },                  { "productRetailerId": "product-SKU-in-catalog" }              ]},              {              "title": "the-section-title",              "product_items": [                 { "productRetailerId": "product-SKU-in-catalog" }                                         ]}       ] |

##### Button

| 参数名 |  类型  | 是否必填 |                             备注                             |   示例   |
| :----: | :----: | :------: | :----------------------------------------------------------: | :------: |
|  type  | String |    Y     |                 按钮类型：当前可选值：reply                  |  reply   |
| title  | String |    Y     | 按钮标题：不能为空字符串、当前消息中必须唯一,支持表情符号，不支持markdown。最大长度:20个字符 | 按钮标题 |
|   id   | String |    Y     | 按钮的唯一标识符。当用户单击按钮时，在webhook中返回此ID。最大长度:256个字符。 |          |

##### Section

|    参数名    |     类型      | 是否必填 |                             备注                             |                             示例                             |
| :----------: | :-----------: | :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|    title     |    String     |    N     | section标题：消息中超过一个section时候必填：最大不超过24字符 |                        You  body text                        |
|     rows     |     Row[]     |    N     | 消息类型为list时必填, 一个Row的list对象,最多为10个详见：Row  | [            {              "id":"unique-row-identifier-here",              "title": "row-title-content-here",              "description": "row-description-content-here",                       }          ] |
| productItems | ProductItem[] |    N     | type=product_list时必填， 产品对象的数组。每个部分至少有1个产品，所有部分最多有30个产品。详见：ProductItem | [                  { "productRetailerId": "product-SKU-in-catalog" },                  { "productRetailerId": "product-SKU-in-catalog" },                            ...              ] |

###### Row

|   参数名    |  类型  | 是否必填 |             备注              |             示例             |
| :---------: | :----: | :------: | :---------------------------: | :--------------------------: |
|    title    | String |    Y     |    标题：最大长度24个字符     |    row-title-content-here    |
|     id      | String |    Y     | 唯一Id标识，最大长度200个字符 |  unique-row-identifier-here  |
| description | String |    N     |             描述              | row-description-content-here |

###### ProductItem

|      参数名       |  类型  | 是否必填 |           备注           | 示例 |
| :---------------: | :----: | :------: | :----------------------: | :--: |
| productRetailerId | String |    Y     | 产品在目录中的唯一标识符 |      |

## RequestBody示例

### 纯文本

```JSON
{
    "account": "IW2578654",
    "wabaId":"110129512080522",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "messageType": "text",
    "body":"Thanks for using TIG WhatsApp."
}
```

### 模板

#### 静态文本

```JSON
{
    "account": "IW9144659",
    "wabaId":"110129512080522",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "messageType":"template",
    "language":"en",
    "templateName": "gogoalshop"
}
```

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=OWM5NWJhYjJkNmU1N2QwM2M2MmJjNTFjMWQ2MjA2ZjJfOUtGRnFTOHZNNnB2Z2NIZDVRMnBmaFJXYWt6a1BpTU1fVG9rZW46Ym94Y25qNjZNN0hPaTMzRVplSkRqWGQ2emRmXzE2ODYxMDQzMjk6MTY4NjEwNzkyOV9WNA)

#### 带Header、按钮（call，visit website）

```JSON
{
    "header":{
        "type":"image",
        "link":"https://www.pianshen.com/thumbs/886/535cfdc357b166dc020c70d0533d85f6.JPEG"
    },
    "bodyParams": ["Lucy"],
    "buttonParams": [
        {
            "type":"text",
            "value":"yHkYRww90" http://xxxxx?type=yHkYRww90
        }
    ],
    "account": "IW2907527",
    "wabaId":"110129512080522",
    "language":"en_US",
    "messageType":"template",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "templateName": "marketing_template"
}
```

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=YzRjNzJjMjE0MTgwNDE4NTdkZDA2MThlODA4N2JmNTdfM0ZhMmo4S1A3NDN2bXlWOFk1NFR4dmh4dmVHblZreHFfVG9rZW46Ym94Y25lSnJNY1ZPUGtBSkF6dmRBODVXbUFnXzE2ODYxMDQzMjk6MTY4NjEwNzkyOV9WNA)

#### 带Header、无按钮

```JSON
{
    "header":{
        "type":"image",
        "link":"https://www.pianshen.com/thumbs/886/535cfdc357b166dc020c70d0533d85f6.JPEG"
    },
    "bodyParams": ["TIG Cinema", "2023-03-17 10:00:00", "F", "66"],
    "account": "IW2907527",
    "wabaId":"110129512080522",
    "language":"en_US",
    "messageType":"template",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "templateName": "sample_movie_ticket_confirmation"
}
```

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=N2VlM2ViMDg2NTk2ZGQ1YjU3Y2JmMTUzYThiYTg0YTlfZ0FIUngwWmZZaUlBTmRZa3pBMXpoTGtadjdjZFZBaDVfVG9rZW46Ym94Y25IcjBDQm9pNmJseWpYdEdSZkx0WXg1XzE2ODYxMDQzMjk6MTY4NjEwNzkyOV9WNA)

#### Header文本，带按钮（2个快捷回复）

```JSON
{
    "account": "IW9144659",
    "wabaId":"110129512080522",
    "bodyParams": ["TIG", "665578"],
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "messageType":"template",
    "language":"en_US",
    "templateName": "tiga_opt"
}
```

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDYyYTdkODJiNzE1MjM5M2U2Yjg0NWViZWE1YTg5MjNfRmFmTzV1dGtITk93MXRaM2FsR05IOTkwSG5hNzNNRXhfVG9rZW46Ym94Y24zTlNWMDR3cVlGbEpQaExrY3huajY4XzE2ODYxMDQzMjk6MTY4NjEwNzkyOV9WNA)

### 图片

```JSON
{
    "account": "IW2578654",
    "wabaId":"110129512080522",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "messageType": "image",
    "media":{
        "link": "http://oss-ycloud-publicread.oss-ap-southeast-1.aliyuncs.com/online/BASE-FILE/2023/03/24/cd56097f-c888-4be5-961d-584d0fa150f3.jpg"
    }
}
```

效果：

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=MmVlNjA2ZjhhZDE4NjM4NDI1NmUxYTljNmJkZTBjMjhfd2V5d0FBVkhnUGJuMmJnS1dMdHZ2azdBUEFoOUJHWXRfVG9rZW46Ym94Y242WlY2T2FDaW1pYzFmYmxtQkN5a1ZkXzE2ODYxMDQzMjk6MTY4NjEwNzkyOV9WNA)

### 视频音频

```JSON
{
    "account": "IW2578654",
    "wabaId":"110129512080522",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "messageType": "video", // 可以改成：audio
    "media":{
        "link": "http://oss-ycloud-publicread.oss-ap-southeast-1.aliyuncs.com/online/BASE-FILE/2023/03/24/xxxx.mp4"
   
    }
}
```

### 文档

```JSON
{
    "account": "IW2578654",
    "wabaId":"110129512080522",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "messageType": "document",
    "media":{
        "link": "http://oss-ycloud-publicread.oss-ap-southeast-1.aliyuncs.com/online/BASE-FILE/2023/03/24/584d0fa150f3.pdf",
        "filename":"WhatsApp_api.pdf"
    }
}
```

### 定位

```JSON
{
    "account": "IW2578654",
    "wabaId":"110129512080522",
    "recipientNumber": "6285234567899",
    "sendNumber": "8618912123456",
    "messageType": "location",
    "location":{
        "latitude": "-5.15161",
        "longitude":"119.41629",
        "name":"Pizza Hut",
        "address":"xxx road No.123"
    }
}
```

效果：

![img](https://rkzav3pcv4.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTE1MjA0Y2I0MTc2YjcwZDg5NTVmZDU3OWM3MzE5MjNfeDVMN1p4NTJGN3kxbmk2VzdwRHRVWjhoZ05vY0xGNjJfVG9rZW46Ym94Y25Xd3dJeFZlUkJyV2xXdzBpWUw0Rjd3XzE2ODYxMDQzMjk6MTY4NjEwNzkyOV9WNA)

### 联系人消息

```JSON
{
    "uid": "2023052500002",//批次号 可选
    "account": "IW2578654",
    "wabaId": "110129512080522",
    "messageType": "contacts",
    "recipientNumber": "8616685177612",
    "sendNumber": "8615121041046",
    "contacts": [
        {
            "addresses": [
                {
                    "street": "XXX街道XX号",
                    "city": "XX市",
                    "state": "XX州",
                    "zip": "553301",
                    "country": "cn",
                    "countryCode": "0086",
                    "type": "WORK"
                }
            ],
            "birthday": "1992-08-13",
            "emails": [
                {
                    "email": "xxx@gmail.com",
                    "type": "WORK"
                }
            ],
            "name": {
                "formattedName": "jack chen",
                "firstName": "chen",
                "lastName": "jack",
                "middleName": "jin",
                "suffix": "jack",
                "prefix": "chen"
            },
            "org": {
                "company": "微软",
                "department": "研发",
                "title": "你好啊"
            },
            "phones": [
                {
                    "phone": "8616685177612",
                    "waId": "28288282",
                    "type": "work"
                }
            ],
            "urls": [
                {
                    "url": "https://www.google.com/",
                    "type": "HOME"
                }
            ]
        }
    ]
}
```

### 互动消息

#### list

```JSON
{
    "uid": "2023052500002",
    "account": "IW2267524",
    "wabaId": "110129512080523",
    "messageType": "interactive",
    "recipientNumber": "8616685177612",
    "sendNumber": "8615121041046",
    "interactive": {
        "type": "list",
        "header": {
            "type": "text",
            "text": "5G Message header"
        },
        "body": {
            "text": "meta Message Text "
        },
        "footer": {
            "text": "meta Message Text Footer"
        },
        "action": {
            "button": "button name",
            "sections": [
                {
                    "title": "meta Message Title1",
                    "rows": [
                        {
                            "id": "20230524001",
                            "title": "title1",
                            "description": "some description"
                        },
                        {
                            "id": "20230524002",
                            "title": "title12",
                            "description": "some description"
                        }
                    ]
                },
                {
                    "title": "meta Message Title2",
                    "rows": [
                        {
                            "id": "20230524003",
                            "title": "title text",
                            "description": "some description"
                        },
                        {
                            "id": "20230524004",
                            "title": "title text1",
                            "description": "some description"
                        }
                    ]
                }
            ]
        }
    }
}
```

#### button

```JSON
{
    "uid": "2023052500002",
    "account": "IW2267524",
    "wabaId": "110129512080532",
    "messageType": "interactive",
    "recipientNumber": "8616685177612",
    "sendNumber": "8615121041046",
    "interactive": {
        "type": "button",
        "header": {
            "type": "text",
            "text": "Meta Message header"
        },
        "body": {
            "text": "Meta Message Text "
        },
        "footer": {
            "text": "Meta Message Text Footer"
        },
        "action": {
            "buttons": [
                {
                    "type": "reply",
                    "reply": {
                        "id": "unique-postback-id",
                        "title": "Button’s Title2"
                    }
                },
                {
                    "type": "reply",
                    "reply": {
                        "id": "unique-postback-id1",
                        "title": "Button’s Title3"
                    }
                }
            ]
        }
    }
}
```

#### product

```JSON
{
    "uid": "2023052500002",
    "account": "IW2267524",
    "wabaId": "110129512080532",
    "messageType": "interactive",
    "recipientNumber": "8616685177612",
    "sendNumber": "8615121041046",
    "interactive": {
        "type": "product",
        "body": {
            "text": "Meta Message Text "
        },
        "footer": {
            "text": "Meta Message Text Footer"
        },
        "action": {
            "catalogId": "202305241564", //必须为真实数据
            "productRetailerId": "2023052415640001" //必须为真实数据
        }
    }
}
```

#### product_list

```JSON
{
    "uid": "2023052500002",
    "account": "IW2267524",
    "wabaId": "110129512080532",
    "messageType": "interactive",
    "recipientNumber": "8616685177612",
    "sendNumber": "8615121041046",
    "interactive": {
        "type": "product_list",
        "header": {
            "type": "text",
            "text": "5G Message header"
        },
        "body": {
            "text": "Meta Message Text "
        },
        "footer": {
            "text": "Meta Message Text Footer"
        },
        "action": {
            "catalogId": "202305241564",
            "sections": [
                {
                    "title": "title1",
                    "productItems": [
                        {
                            "productRetailerId": "product-SKU-in-catalog" //必须为真实数据
                        },
                        {
                            "productRetailerId": "product-SKU-in-catalog2"//必须为真实数据
                        }
                    ]
                },
                {
                    "title": "title2",
                    "productItems": [
                        {
                            "productRetailerId": "product-SKU-in-catalog"//必须为真实数据
                        },
                        {
                            "productRetailerId": "product-SKU-in-catalog2"//必须为真实数据
                        }
                    ]
                }
            ]
        }
    }
}
```

## Response

| 参数名  |  类型  |     备注     |            示例             |
| :-----: | :----: | :----------: | :-------------------------: |
|  code   | String | 错误码0-成功 |             143             |
| message | String |   错误消息   | WhatsApp Tempalte Not Found |
|  data   | Object | 数据体消息Id |     335259505606656000      |

Response示例

```JSON
{
    "code": "0",
    "message": "提交成功",
    "data": "335259505606656000"
}
```

# 接口参数签名规则

1. 将请求头参数名nonce及所有请求体参数名（key）以ASCII码表顺序升序排列；
2. 排序后把参数值非空的参数名（key）参数值（value）依次进行字符串拼接，拼接处不包含其他字符；
3. 拼接成包含kv的长串后，在该长串的尾部拼接账号的password，得到签名字符串的组装；
4. 最后对签名字符串，使用MD5算法加密后，得到MD5加密密文后转为小写，即为请求头sign值

### 签名生成示例

```Java
package com.tig.sign;

import com.alibaba.fastjson.JSONObject;
import org.apache.commons.lang3.StringUtils;
import org.springframework.util.DigestUtils;

import java.util.Map;
import java.util.TreeMap;


public class SignUtil {
   private static final String SIGN = "sign";

   /**
    * 发送加签
    */
   public static String generateSign(String password, TreeMap<String, String> paramMap) {
       paramMap.remove(SIGN);
       StringBuilder sb = new StringBuilder();
       for (Map.Entry<String, String> entry : paramMap.entrySet()) {
           if (StringUtils.isNotBlank(entry.getValue())) {
               sb.append(entry.getKey());
               sb.append(entry.getValue());
           }
       }
       return DigestUtils.md5DigestAsHex((sb.toString() + password).getBytes()).toLowerCase();
   }

     public static void main(String[] args) {
        /**发送加签验签*/
        String password = "4Z7bMS1eLI6895";
       
        WhatsAppSubmit request = new WhatsAppSubmit();
        request.setAccount("IW9144659");
        request.setSendNumber("8618912123456");
        request.setRecipientNumber("6285234567899");
        request.setTemplateName("tig_otp");
        request.setLanguage("en_US");
        
        List<WaParameter> bodyParams = new ArrayList<>();
        bodyParams.add(new WaParameter("text", "TIG"));
        bodyParams.add(new WaParameter("text", "245098"));
        
        request.setBodyParams(bodyParams);
        
        TreeMap<String, String> treeMap = JSON.parseObject(JSON.toJSONString(request), new TypeReference<TreeMap<String, String>>() {
        });
        treeMap.put("nonce", "123456789");

       System.out.println(JSONObject.toJSONString(treeMap));
       String sign = generateSign(password, treeMap);
       System.out.println(sign);
   }
}
```

# 附：Language Code List

| Code  |   DisplayName    |
| :---: | :--------------: |
|  af   |    Afrikaans     |
|  sq   |     Albanian     |
|  ar   |      Arabic      |
|  az   |   Azerbaijani    |
|  bn   |     Bengali      |
|  bg   |    Bulgarian     |
|  ca   |     Catalan      |
| zh_CN |  Chinese (CHN)   |
| zh_HK |  Chinese (HKG)   |
| zh_TW |  Chinese (TAI)   |
|  hr   |     Croatian     |
|  cs   |      Czech       |
|  da   |      Danish      |
|  nl   |      Dutch       |
|  en   |     English      |
| en_GB |   English (UK)   |
| en_US |   English (US)   |
|  et   |     Estonia      |
|  fil  |     Filipin      |
|  fi   |     Finnish      |
|  fr   |      French      |
|  de   |      German      |
|  el   |      Greek       |
|  gu   |     Gujarati     |
|  ha   |      Hausa       |
|  he   |      Hebrew      |
|  hi   |      Hindi       |
|  hu   |    Hungarian     |
|  id   |    Indonesian    |
|  ga   |      Irish       |
|  it   |     Italian      |
|  ja   |     Japanese     |
|  kn   |     Kannada      |
|  kk   |      Kazakh      |
|  ko   |      Korean      |
|  lo   |       Lao        |
|  lv   |     Latvian      |
|  lt   |    Lithuanian    |
|  mk   |    Macedonian    |
|  ms   |      Malay       |
|  ml   |    Malayalam     |
|  mr   |     Marathi      |
|  nb   |    Norwegian     |
|  fa   |     Persian      |
|  pl   |      Polish      |
| pt_BR | Portuguese (BR)  |
| pt_PT | Portuguese (POR) |
|  pa   |     Punjabi      |
|  ro   |     Romanian     |
|  ru   |     Russian      |
|  sr   |     Serbian      |
|  sk   |      Slovak      |
|  sl   |    Slovenian     |
|  es   |     Spanish      |
| es_AR |  Spanish (ARG)   |
| es_MX |  Spanish (MEX)   |
| es_ES |  Spanish (SPA)   |
|  sw   |     Swahili      |
|  sv   |     Swedish      |
|  ta   |      Tamil       |
|  te   |      Telugu      |
|  th   |       Thai       |
|  tr   |     Turkish      |
|  uk   |    Ukrainian     |
|  ur   |       Urdu       |
|  uz   |      Uzbek       |
|  vi   |    Vietnamese    |
|  zu   |       Zulu       |
