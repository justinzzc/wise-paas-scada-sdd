# Notificaion

### DB Schema {#db-schema}

* smtp\_config

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| smtp\_id |  |  |  |  |  |  |
| host | integer | Y | Y | Y | Y |  |
| port | varchar\(128\) | Y |  |  |  | UNIQUE |
| secure | integer | Y |  |  |  | {1: email, 2:line, 3: wechat } |
| user\_name | Array \(varchar\(256\)\[\]\) |  |  |  |  |  |
| password |  |  |  |  |  | crypto-js https://github.com/brix/crypto-js |
| from\_mail\_addr |  |  |  |  |  |  |
| subject |  |  |  |  |  |  |

* notification\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| notification\_id | integer | Y | Y | Y | Y |  |
| name | varchar\(128\) | Y |  |  |  | UNIQUE |
| type | integer | Y |  |  |  | {1: email, 2:line, 3: wechat } |
| send\_list | Array \(varchar\(256\)\[\]\) |  |  |  |  |  |
| smtp\_id |  |  |  |  |  |  |

* 限制

  * sys\_parameters

    * NOTIFICATION\_MAX\_COUNT: 32

  * scope

    * 加上system\_setting

    * migrate裡也要加，參考manager\_alarm

  * others

    * sendList的上限也是32


