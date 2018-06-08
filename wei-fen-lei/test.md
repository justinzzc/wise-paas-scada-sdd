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
  * 15\#

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
| instance\_launched | boolean | Y |  | 是否透過eventManager啟動event instance |  | default:false |
| ref\_text\_value | varchar\(256\) | N |  | 文字參考值 |  |  |

### [API](#event-log-api) {#event-log-api}

* \[POST\] /EventLogs

  * 包含紀錄測點一起帶給這支api
  * 紀錄測點為array, 可以為空
  * user\(token\)對選定的**事件點、參考點、多個記錄點**都需要有deviceRight，否則無法新增\(權限不足\)

* \[GET\] /EventLogs/list

  * 取得列表，只回傳eventId/eventName/scadaId/description/instanceLaunched
  * 只取出事**件點、參考點、多個記錄點**都有deviceRight的events

* \[GET\] /EventLogs/list/{scadaId}

  * 取得相同scada的event列表，只回傳eventId/eventName/scadaId/description/instanceLaunched
  * 只取出事**件點、參考點、多個記錄點**都有deviceRight的events

* \[GET\] /EventLogs/info/{eventId}
  * 取得單一事件的設定細節，包含記錄測點
  * 只取出事**件點、參考點、多個記錄點**都有deviceRight的event, 否則回傳權限錯誤
* \[DELETE\] /EventLogs/{eventId}
  * 刪除事件測點及其記錄測點
  * 只刪除**事件點、參考點、多個記錄**點都有deviceRight的event, 否則回傳權限錯誤
* \[POST\] /EventLogs/syncInstance
  * 選擇哪一個eventId要launch或close worker裡的event instance
  * 只同步**事件點、參考點、多個記錄點**都有deviceRight的event, 否則回傳權限錯誤
* \[POST\] /EventLogs/data
  * 取得紀錄點的值
  * 只能取出**事件點、參考點、多個記錄點**都有deviceRight的event data, 否則回傳權限錯誤

### Note

* **刪除event會做的事情**

  * 如果deleteData為true
    * 會刪掉config/instance/data
  * 如果deleteData為false
    * 會刪掉config/instance

* **eventLogRecord \(Array\)在insert/update的差別**

  * Insert
    * null/empty arr在insert的話都是相同結果
  * Update
    * empty arr "\[\]"
      * 會把原本的records\(如果有\)都刪掉
    * null arr
      * 不對紀錄測點做任何變動

* **permission**

  * user\_allow\_device
    * 要檢查到device層級
  * 所有API都使用manage\_event \(scope\)

---

### FAQ



