# Notificaion

### TODO {#db-schema}

1. 改寫SCHEMA
2. 讓STVEN檢查DB SCHEMA和API INTERFACE

### DB Schema {#db-schema}

* group\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| group\_id | integer | Y | Y | Y | Y |  |
| name | varchar\(128\) | Y |  |  |  | UNIQUE |
| description | varchar\(256\) | N |  |  |  |  |
| type | integer | Y |  |  |  | {1: email, 2:line, 3: wechat } |
| send\_list | Array \(jsonb\) | N |  |  |  | 下面有example |
| config | jsonb | N |  |  |  | 下面有example |

* send\_list

```
[{firstName: "test1", lastName: "ccc", sendTarget: "werwer@werwer.werw"}, 
{firstName: "test1", lastName: "ccc", sendTarget: "werwer@werwer.werw"}]
```

* config example \(smtp\)

```
{
"host":"smtp.gmail.com",
"port":465,
"secure":true,
"user":"scada.notify@gmail.com",
"pass":"1qaz@WSX3edc"
"FromMailAddr":"SCADA Notification <scada.notify@gmail.com>",
"subject": "test"
}
```

* password加解密

  * | crypto-js [https://github.com/brix/crypto-js](https://github.com/brix/crypto-js) |
    | :--- |

* dao function

  * create notify

  * get notify

  * update notify

  * delete notify

  * create smtp\_config

  * get smtp\_config

  * update smtp\_config

  * delete smtp_config_

* api

  * POST /notification - 建立notification

  * GET /notification - 取出notifications

  * PUT /notification/{notification\_id} - 更新notification

  * DELETE / notification/{notification\_id} 刪除notification

  * POST /notificationConfig - 建立notificationConfig  
     \(透過api限制一種type在table只能有一筆record\)

  * GET /notificationConfig - 取出notificationConfig

  * PUT /notificationConfig/{config\_id} - 更新notificationConfig

  * 不開放刪除notificationConfig  api

  * POST /notification/send 送出通知 \(要在body裡帶notification\_id\)

    * notification\_id array, ex\[1,2,3,4\]
    * 討論一下流程

  * email sen測試通知的api 第一階段先不做嗎

    * 測試的api要多帶收的人email

* 限制

  * 是否一樣要設上限值?

    * 通知的數量 \(scada portal是訂32\)

    * send list的數量 \(scada portal是訂32\)

  * 一樣在notification schema新增一個sys\_parameter資料表存這些?

* 如果一開始都沒有setting \(line\)，那就再第一個人trigger時建立

* TODOS

  * dbmigrate在一開始就先觸發



