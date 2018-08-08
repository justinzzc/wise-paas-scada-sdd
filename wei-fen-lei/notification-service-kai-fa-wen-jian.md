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

* api

  * POST /Group - 建立Group

  ```
  {
      name: "test",
      description: "test",
      type: 1,
      send_list: [
          {firstName: "test1", lastName: "ccc", sendTarget: "werwer@werwer.werw"}, 
          {firstName: "test1", lastName: "ccc", sendTarget: "werwer@werwer.werw"}
      ],
      config: {
          "host":"smtp.gmail.com",
          "port":465,
          "secure":true,
          "user":"scada.notify@gmail.com",
          "pass":"1qaz@WSX3edc"
          "FromMailAddr":"SCADA Notification <scada.notify@gmail.com>",
          "subject": "test"
      }
  }
  ```

  * GET /Group - 取出Group

  * PUT /Group/{group\_id} - 更新Group

  * DELETE / Group/{group\_id} 刪除Group

  * POST /Group/send 送出通知 \(要在body裡帶group\_id\)

    * subject可不帶，但config裡就要設定，兩者都沒時，報錯

  * ```
    [{
      "groupId": 1,
      "msg": "test",
      "subject": "test"
    },{
      "groupId": 2,
      "msg": "test"
    }]
    ```



