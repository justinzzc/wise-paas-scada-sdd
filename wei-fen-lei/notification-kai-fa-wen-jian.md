# Notificaion

### DB Schema {#db-schema}

* notification\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| notification\_id | integer | Y | Y | Y | Y |  |
| name | varchar\(64\) | Y |  |  |  | UNIQUE |
| type | integer | Y |  |  |  | {1: line, 2: email, 3: wechat } |
| send\_list | Array | Y |  |  |  |  |

* alarm\_notification

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y | Y |  |  |  |
| notification\_id | integer | Y | Y |  |  |  |

* sys\_parameters

  * NOTIFICATION\_MAX\_COUNT: 32

* scope

  * 加上system\_setting

  * migrate裡也要加，參考manager\_alarm

  * 



