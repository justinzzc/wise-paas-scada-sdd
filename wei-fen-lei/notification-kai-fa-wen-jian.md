# Notificaion

### DB Schema {#db-schema}

* notification\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| host | string | Y | Y | Y | Y |  |
| port | integer |  |  |  |  |  |
| secure | boolean |  |  |  |  |  |
| user\_name | string |  |  |  |  |  |
| password | string\(md5\) |  |  |  |  | 加密方式1. md5, 2. jwt token with secret |
| from\_mail\_addr | string |  |  |  |  |  |
| subject | string |  |  |  |  |  |

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



