# Event log開發文件

### DB Schema

* event\_log\_record \(紀錄測點\)
  * 3\#

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| event\_id | integer | Y | Y |  | Y |  |
| device\_id | varchar\(256\) | Y | Y | 紀錄測點的設備識別名 | Y |  |
| tag\_name | varchar\(128\) | Y | Y | 紀錄測點名稱 | Y |  |

* event\_log\_list \(事件測點和參考測點\)
  * 13\#

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| event\_id | integer | Y | Y |  | Y | AUTO\_INCREMENT |
| event\_name | varchar\(128\) | Y |  | 事件紀錄名稱 |  |  |
| scada\_id | varchar\(36\) | Y |  | 事件測點的節點識別碼 |  |  |
| description | varchar\(256\) | N |  | 事件描述 |  |  |
| device\_id | varchar\(256\) | Y |  | 事件測點的設備識別名 |  |  |
| tag\_name | varchar\(128\) | Y |  | 事件測點名稱 |  |  |
| event\_type | integer | Y |  | 事件類型 |  | {1:&gt;=參考值, 2:&lt;=參考值, 3:==參考值, 4:&gt;=參考測點, 5:&lt;=參考測點, 6:==參考測點, 7:依取樣間隔紀錄} |
| ref\_value | double | N |  | 參考值 |  |  |
| ref\_device\_id | varchar\(256\) | N |  | 參考測點的設備識別名 |  |  |
| ref\_tag\_name | varchar\(128\) | N |  | 參考測點名稱 |  |  |
| sample\_interval | integer | Y |  | 取樣間隔 |  |  |
| sample\_unit | integer | Y |  | 取樣間隔單位 |  | value: {1:秒, 2:分, 3:小時} |
| sample\_amount | integer | Y |  | 事件之後紀錄之取樣數量 |  | 值如果為0，代表「持續記錄」 |

### [API](#event-log-api) {#event-log-api}

* **create**

  * \[POST\] /EventLogs
    * 包含紀錄測點一起帶給這支api
    * 紀錄測點為array, 可以為空
      * 空的話就只存本身\(事件測點\)的設定

* **read**

  * \[GET\] /EventLogs/list
    * 取得列表，只回傳eventId/eventName/scadaId/description
  * \[GET\] /EventLogs/info/{eventId}
    * 取得單一事件細節，包含記錄測點
  * \[POST\] /EventLogs/data
    * 取得紀錄點的值

* **update**

  * \[PUT\] /EventLogs/{eventId}
    * 更新特定事件，包含記錄測點
    * 事件測點不能修改，所以不用傳scadaId/deviceId/tagName
    * 回傳值要改成obj 包含event\_id，stacy那邊需要

* **delete**

  * \[DELETE\] /EventLogs/{eventId}
    * 刪除事件測點及其記錄測點

### Note

* **刪除或修改的的連動情境**

  * \[PUT\] /EventLogs/{eventId}
    * 修改事件名稱\(event\_name\)
      * mongo要根據eventId更新data的事件名稱
        * 不然改完名後的舊資料會撈不到
      * 不用再去更新event\_name，因為stacy那邊會刪除再新增
  * \[DELETE\] /EventLogs/{eventId}
    * portal API刪除事件
      * mongo要根據eventId刪除相關資料
    * portal API刪除紀錄測點
      * mongo要根據eventId/device\_id/tag\_name刪除相關資料
        * 同上，不刪除相關資料會抓到舊資料

* **eventLogRecord \(Array\)在insert/update的差別**

  * Insert
    * null/empty arr在insert的話都是相同結果
  * Update
    * empty arr "\[\]"
      * 會把原本的records\(如果有\)都刪掉
    * null arr
      * 不對紀錄測點做任何變動

* **request validation**

  * type要對/數量不能少/數量不能多
  * 驗證相關測點或事件是否真的存在
  * 測點間的關係驗證，例如離散點的參考點就要是離散點之類的

* **user permission**

  * user\_allow\_device
    * 只要檢查到scada\_id的層級就好
  * validScope - EditConfig
    * Eventlog.createEventLog
    * Eventlog.updateEventLog
    * Eventlog.deleteEventLog
  * validToken
    * Eventlog.listEventLogInfoByScadaIdAndEventName
    * Eventlog.listAllEventLogBasic
  * validScope - GetValue
    * Eventlog.getEventLogData

* [把eventManager跟config setting分開](#event-log-test) {#event-log-test}

  * 我會在event\_log\_list加一欄位:instance\_launched \(boolean, default: false\)
    * set true: 開啟或重啟
    * set false: 關閉
  * portal新增API
    * POST /EventLogs/syncInstance
      * type: launch/close
    * DELETE /EvnetLogs/deleteData/{eventId}

---

### TODO



