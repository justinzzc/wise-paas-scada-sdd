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
| compare\_text | varchar\(256\) |  |  | 預留給文字點 |  |  |

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

* API
  * POST /Alarms
  * GET /Alarms
  * GET /Alarms/{alarm\_id}
  * POST /Alarms/log
  * POST /Alarms/status
    * 也可以放在status裡

### TODO

* dbmigrate
  * 開個branch
* dbmanager
  * dao
    * createAlarm
    * getAlarm
    * updateAlarm
    * deleteAlarm

### FAQ \(archived\)

* 什麼是alarm code?

  * 因為有些設備是有自訂的alarm code 想說讓她有個相互對應

* notification相關問題

  * 先不做通知, db schema不預留

* 文字點是否要做?

  * 不用

### FAQ \(on Progress\)

* 要做update，要討論下那些欄位可以動那些不行?

* current alarm list裡，只秀tag就好了嗎?這樣會不會分不清是哪個scada/device?

* 舊的那一套alarm 機制是不是就棄用了, 像是離散有8個priority....類比有hh/ll等等的

  * 改成很單純的六種condition配合user自己設定的upper and lower limit
  * 之後有需要把舊的schema刪掉嗎

* alarm ack的那個頁面 有要做order嗎?

* 有要做刪除alarm嗎?

* 需要作連動刪除?

  * 例如tag刪除，是否要刪除alarm config \(把alarm config裡的該tag拿掉\), 並更新worker

* 新增完config要直接啟動alarm instance嗎? 還是要像eventlog一樣，讓使用者可以控制



