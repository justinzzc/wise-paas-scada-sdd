# Notificaion

### DB Schema {#db-schema}

* smtp\_config

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| smtp\_id | integer | Y | Y | Y | Y |  |
| host | varchar\(256\) | Y |  |  |  |  |
| port | integer | Y |  |  |  |  |
| secure | boolean | Y |  |  |  |  |
| user\_name | varchar\(128\) | Y |  |  |  |  |
| password | varchar\(256\) 因為是存加密後的結果，長度較長 | Y |  |  |  | crypto-js [https://github.com/brix/crypto-js](https://github.com/brix/crypto-js) |
| from\_mail\_addr | varchar\(256\) | Y |  |  |  |  |
| subject | varchar\(256\) | N |  |  |  |  |

* notification\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| notification\_id | integer | Y | Y | Y | Y |  |
| name | varchar\(128\) | Y |  |  |  | UNIQUE |
| type | integer | Y |  |  |  | {1: email, 2:line, 3: wechat } |
| send\_list | Array \(varchar\(256\)\[\]\) | N |  |  |  |  |
| smtp\_id | integer | N |  |  |  |  |

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

  * POST /smtpConfig - 建立smtpConfig  
     \(第一階段限制該table只能一組config\)

  * GET /smtpConfig - 取出smtpConfig

  * PUT /smtpConfig/{smtp\_id} - 更新smtpConfig

  * 第一階段不開放刪除smtp config api

  * POST /notification/send 送出通知 \(要在body裡帶notification\_id\)

  * 測試通知的api 第一階段先不做嗎

* 限制

  * 是否一樣要設上限值?

    * 通知的數量 \(scada portal是訂32\)

    * send list的數量 \(scada portal是訂32\)

  * 一樣在notification schema新增一個sys\_parameter資料表存這些?



