# Alarm 開發文件

### DB Schema \(config\) {#db-schema}

* alarm\_list

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y |  | 不加入PK, 而是用UNIQUE, 因為設PK會造成code+message重覆 | Y | \* auto\_increment \* UNIQUE |
| code | varchar\(16\) | Y | Y |  |  |  |
| message | varchar\(256\) | Y | Y |  |  |  |
| condition\_type | integer | Y |  |  |  | {1: above, 2: below, 3: equal, 4: out range, 5: in range} |
| lower\_limit | double |  |  |  |  |  |
| upper\_limit | double |  |  |  |  |  |
| compare\_text | varchar\(256\) |  |  | ??使否要預留 |  |  |

* alarm\_tag

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y | Y |  | Y |  |
| scada\_id | varchar\(36\) | Y | Y |  | Y |  |
| device\_id | varchar\(256\) | Y | Y |  | Y |  |
| tag\_name | varchar\(128\) | Y | Y |  | Y |  |

### SPEC

* condition
  * above
    * 只存upper
  * below
    * 只存lower
  * in range
    * 兩個都要存
  * equal
    * upper/lower都存一樣的
  * out range
    * 兩個都要存
* "Current Alarm List" 和 首頁的"alarm count"透過stacy給的同一個function拿到

* 在UI和API去擋code+message不可重複
  * 但UI沒辦法做，只能從api做
* 權限如何做?
  * check right到device
  * alarm setting頁面裡，拿掉沒權限的tag就好
  * log/status都是從stacy那邊拿東西回來後在過濾權限

### FAQ \(archived\)

* 什麼是alarm code?
  * 因為有些設備是有自訂的alarm code 想說讓她有個相互對應

* notification相關問題

  * 先不做通知, db schema不預留

* 文字點是否要做?

  * 不用

### FAQ \(on Progress\)

* 要做update嗎?

  * 如果要做，需要討論下那些欄位能改或不能改. 

* current alarm list裡，只秀tag就好了嗎?這樣會不會分不清是哪個scada/device?

* 舊的那一套alarm 機制是不是就棄用了, 像是離散有8個priority....類比有hh/ll等等的

  * 改成很單純的六種condition配合user自己設定的upper and lower limit
  * 之後有需要把舊的schema刪掉嗎

* 是否要預留TEXT比較欄位?

* alarm ack的那個頁面 有要做filter嗎?



