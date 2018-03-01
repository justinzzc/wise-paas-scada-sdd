# Event log開發文件

### DB Schema

* event\_log\_record \(紀錄測點\)
  * 4\#

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar\(128\) | Y | Y | 事件紀錄名稱 | Y |  |
| scada\_id | varchar\(36\) | Y | Y | 事件測點的節點識別碼 \(同紀錄測點\) | Y |  |
| device\_id | varchar\(256\) | Y | Y | 紀錄測點的設備識別名 | Y |  |
| tag\_name | varchar\(128\) | Y | Y | 紀錄測點名稱 | Y |  |

* event\_log\_list \(事件測點和參考測點\)
  * 12\#

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar\(128\) | Y | Y | 事件紀錄名稱 | Y |  |
| scada\_id | varchar\(36\) | Y | Y | 事件測點的節點識別碼 | Y |  |
| description | varchar\(256\) | N |  | 事件描述 |  |  |
| device\_id | varchar\(256\) | Y |  | 事件測點的設備識別名 |  |  |
| tag\_name | varchar\(128\) | Y |  | 事件測點名稱 |  |  |
| event\_type | integer | Y |  | 事件類型 |  | 值:{1:&gt;=,2:&lt;=,3:“&gt;= tag value,4:“&lt;= tag value} |
| ref\_value | double | N |  | 參考值 |  |  |
| ref\_device\_id | varchar\(256\) | N |  | 參考測點的設備識別名 |  |  |
| ref\_tag\_name | varchar\(128\) | N |  | 參考測點名稱 |  |  |
| sample\_interval | integer | Y |  | 取樣間隔 |  |  |
| sample\_unit | integer | Y |  | 取樣間隔單位 |  | value: {1:秒, 2:分, 3:小時} |
| sample\_amount | integer | Y |  | 事件之後紀錄之取樣數量 |  | 值如果為0，代表「持續記錄」 |

### [API](#event-log-api) {#event-log-api}

* **create**

  * \[POST\] /eventLogs - Eventlog.createEventLog
    * 包含紀錄測點一起帶給這支api
    * 紀錄測點為array, 可以為空
      * 空的話就只記錄本身\(事件測點\)的值
  * addRecordTag

* **read**

  * \[GET\] /eventLogs/info - Eventlog.listAllEventLogInfo
    * 得到所有的事件紀錄，會附加每個event的詳細資訊

* **update**

  * updateEventLog
    * 更新事件紀錄的設定細節
  * event name可以修改
  * 事件測點不要修改 \(目前\)
    * 應該說除了事件測點不能改之外，其他都可以改

* **delete**

  * deleteEventLo
    * 據事件紀錄名稱來刪除事件
  * deleteRecordTag
    * 刪除某事件底下的所有紀錄測點

### Note

* 刪除或修改的的連動情境
  * 修改
    * 修改事件名稱\(event\_name\)
      * mongo要根據scadaid和event\_name更新data的事件名稱
        * 不然改完名後的舊資料會撈不到
      * 修改事件測點的eventname要一併把所屬的紀錄測點的event\_name更新
  * 刪除
    * portal API刪除事件
      * mongo要根據scadaid和event\_name刪除相關資料
        * 不然之後若有同scada\_id+event\_name的資料會撈出舊資料
    * portal API刪除紀錄測點
      * mongo要根據scadaid/event\_name/device\_id/tag\_name刪除相關資料
        * 同上，不刪除相關資料會抓到舊資料
      * 可是現在開給Eryn的update api是刪掉所屬的記錄測點再新增一次

### TODO

* scada-dbmanager
* flyway
* event log存取權限
  * get\_value
  * 編輯先pending \(待討論\)
* 確認event\_type與參考值/參考測點的關聯是否正確



