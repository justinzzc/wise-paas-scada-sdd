# Notificaion

### DB Schema {#db-schema}

* group\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| group\_id | varchar\(12\) | Y | Y | Y | Y | 長度12/小寫/英文+數字 |
| name | varchar\(128\) | Y |  |  |  | UNIQUE |
| description | varchar\(256\) | N |  |  |  |  |
| type | integer | Y |  |  |  | {1: email, 2:line, 3: wechat } |
| config | jsonb | N |  |  |  |  |
| send\_list | Array \(jsonb\) | N |  |  |  | 下面有example |

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

### dbmanager

* dao function

  * create notify

  * get notify

  * update notify

  * delete notify

  * create smtp\_config

  * get smtp\_config

  * update smtp\_config

  * delete smtp_config_

### API

* POST /Group - 建立Group
  * type是email\(1\)的話就要填config裡相關欄位, line\(2\)/wechat\(3\)就不用帶config
  * sendTarget依照type帶不同的值, 像是email addr/ token...

```
// request body
{
  "name": "string(128)",
  "description": "string(256)",
  "type": number(1|2|3),
  "config": {
    "host": "string(256)",
    "port": 0,
    "secure": true,
    "username": "string(128)",
    "password": "string(128)",
    "senderEmail": "string(256)",
    "emailSubject": "string(256)"
  },
  "sendList": [
    {
      "firstName": "string(32)",
      "lastName": "string(32)",
      "sendTarget": "string(256)"
    }
  ]
}
```

* GET /Group - 取出Group

  * 安全考量不會回傳password

* PUT /Group/{group\_id} - 更新Group

  * 規則跟insert一樣

  * config欄位一定要全帶或都不帶

  * sendList一定要全帶或都不帶

  * 其他欄位有改到再帶

  * config.password 有改到再帶

    * 這個UI要不要加上確認新密碼功能? 要找和益討論

* DELETE / Group/{group\_id} 刪除Group

* POST / Group/test

  * 這跟當初討論的不太依樣，當初是說test/send都在同一支就好

  * 但流程上，user是在group建立前就要測試了，所以沒有goupId可以測

  ```
  {
    "type": 1,
    "sendList": []
  }
  ```

* POST /Group/send 送出通知 \(要在body裡帶group\_id\)

  * subject可不帶，但config裡就要設定，兩者都沒時，報錯

  ```
  // request body
  [{
    "groupId": 1,
    "msg": "test",
    "subject": "test"
  },{
    "groupId": 2,
    "msg": "test"
  }]
  ```

### TODO

* 密碼安全機制

  * [https://github.com/brix/crypto-js\#aes-encryption](https://github.com/brix/crypto-js#aes-encryption)

  * 存進db前先加密, 取出時再解密

  * GET /Group 時不給password

  * 但Update時要怎麼做?

  * 換smtp密碼時 要有密碼確認

* OTHERS

  * dbmigrate

  * SSO

  * PCF

* createAt/editAt

* 把SRP從SSO刪除，並再pcf重新跑過整個流程srp register sso

* url 叫作

  portal-notification.wise-paas.com

* 連同scada一起改sso規則

* 跟stacy

  * 有辦法把line的response傳出來嗎 因為可能要回報給user, 例如token不合法之類的
  * email送失敗，我是用smtp.gmail.com這組測的
  * uility要有我ssoToken存mongo的部分

* 把srp name改成notification

  * 刪掉原本的

* 加上TSL/SSL機制

* postgres不能存特殊符號 例如 roy's line



