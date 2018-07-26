# Notificaion

### DB Schema {#db-schema}

* notification\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| notification\_id | integer | Y | Y | Y | Y |  |
| name | varchar\(128\) | Y |  |  |  | UNIQUE |
| type | integer | Y |  |  |  | {1: email, 2:line, 3: wechat } |
| send\_list | Array \(varchar\(256\)\[\]\) |  |  |  |  |  |

* alarm\_notification

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y | Y |  |  |  |
| notification\_id | integer | Y | Y |  |  |  |
| trigger\_type | integer | Y | Y |  |  | {1: happend} |

* sys\_parameters

  * NOTIFICATION\_MAX\_COUNT: 32

* scope

  * 加上system\_setting

  * migrate裡也要加，參考manager\_alarm

* others

  * sendList的上限也是32



