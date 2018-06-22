# Alarm 開發文件

### DB Schema \(config\) {#db-schema}

* alarm\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y | Y | Y | Y |  |
| scada\_id | varchar\(36\) | Y |  |  |  |  |
| code | varchar\(16\) | Y |  |  |  | code在同一scada下不能重覆，在程式裡檔 |
| message | varchar\(256\) | Y |  |  |  |  |
| condition\_type | integer | Y |  |  |  | {1: above, 2: below, 3: equal, 4: in range, 5: out range} |
| lower\_limit | double |  |  |  |  |  |
| upper\_limit | double |  |  |  |  |  |
| instance\_launched | boolean | Y |  |  |  | default: false |

* alarm\_tag

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y | Y |  | Y |  |
| device\_id | varchar\(256\) | Y | Y |  | Y |  |
| tag\_name | varchar\(128\) | Y | Y |  | Y |  |

### SPEC

* **code/message規則                                          
  **

  * 同一個scada下的code+message pair不能重覆

  * 不同scada下可以定義相同的code+message pari

* **condition**

  * above \(大於等於某個值，就觸發警報\)
    * if \(x &gt;= upperLimit\) { return true }
  * below \(小於等於某個值，就觸發警報\)
    * if \(x &lt;= lowerLimit\) { return true }
  * equal
    * if \(x === lowerLimit && x === upperLimit\)
  * in range

    * if \(x &gt; lowerLimit && x &lt; upperLimit\) { return true }

  * out range

    * if \(x &gt; upperLimit \|\| x &lt; lowerLimit\) { return true}

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
  * PUT /Alarms/{alarm\_id}
    * 如果要更新的alarm已經是launche，則call stacy做的upate
    * 如果要更新的alarm不是launch狀態ˋ，則指更新cfg, lauch狀態保留給使用者自己呼叫起來
  * POST /Alarms/log
  * POST /Alarms/status
    * 跟eryn討論看看前端能不能只call一次就好，降低effort
  * * 也可以放在status裡
  * POST /Alarms/sync

* stacy 做的delete alarm支援三種

  * \[{scadaId: 'xxxx'}\]
    * 刪除整個instance
  * \[{scadaId:'xxxx', alarmId: 'xxxx'}\]
    * 刪除整個instance
  * \[{scadaId: 'xxx', alarmId: 'xxx', tags: \[{deviceId:'xxx', tagName: 'xxx'}\]}\]
    * update用途，把tag移除該alarm的監控
    * 只改tags,沒改config
    * 如果是config/tags都有改, 就直接call update
      * config/tags一起帶

* filter

  * alarm log
    * 選tag就要給device
    * 至少要給scadaId
      * 全部給太多了
    * options
      * project/scada
      * project/scada/device
      * project/scada/device/tag
      * project/scada/alarmId
  * alarm status
    * 選tag就要給devcie
    * 可以都不給

* 連動刪除

  * 刪除project或scada也會刪除下面的alarm

  * 跟event一樣

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

* 要做update，要討論下那些欄位可以動那些不行?

  * alarmId/scadaId

* current alarm list裡，只秀tag就好了嗎?這樣會不會分不清是哪個scada/device?

  * 要能區別，所以API要scada/device資訊

* 舊的那一套alarm 機制是不是就棄用了, 像是離散有8個priority....類比有hh/ll等等的

  * 改成很單純的六種condition配合user自己設定的upper and lower limit

  * 新版上線後，再把舊的拿掉

* alarm ack的那個頁面 有要做order嗎?

  * 先不用，固定排序

* 有要做刪除alarm嗎?

  * 要，設備要去刪，worker也要刪

* 需要作連動刪除?

  * 要連動把alarm\_tag裡的tag刪掉

  * worker的東西也要同步

* 新增完config要直接啟動alarm instance嗎? 還是要像eventlog一樣，讓使用者可以控制

  * yes

* 刪除config要刪除log嗎?

  * log一律都不要刪，但為了讓record有意義，所以保留code+message

* 相同alarmId/scadaId的config\(但可能lowerLimit/upperLimit不一樣\)重複insert，你那邊會怎處理？新的蓋舊的？還是？

  * 不處理 insert一樣的 不會更新

### FAQ \(on Progress\)



