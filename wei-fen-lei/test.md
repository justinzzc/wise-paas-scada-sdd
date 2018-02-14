# Event log開發文件

### DB Schema

* event\_log\_record \(紀錄測點\)
  * 5\#

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar\(128\) | Y | Y | 事件紀錄名稱 | Y |  |
| scada\_id | varchar\(36\) | Y | Y | 事件測點的節點識別碼 | Y |  |
| record\_scada\_id | varchar\(36\) | Y | Y | 紀錄測點的節點識別碼 | Y |  |
| record\_device\_id | varchar\(256\) | Y | Y | 紀錄測點的設備識別名 | Y |  |
| record\_tag\_name | varchar\(128\) | Y | Y | 紀錄測點名稱 | Y |  |

* event\_log\_list \(事件測點和參考測點\)
  * 12\#

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar\(128\) | Y | Y | 事件紀錄名稱 | Y |  |
| scada\_id | varchar\(36\) | Y | Y | 事件測點的節點識別碼 | Y |  |
| event\_description | varchar\(256\) | N |  | 事件描述 |  |  |
| device\_id | varchar\(256\) | Y |  | 事件測點的設備識別名 |  |  |
| tag\_name | varchar\(128\) | Y |  | 事件測點名稱 |  |  |
| event\_type | integer | Y |  | 事件類型 |  | 值:{1,2,3,4} |
| ref\_value | double | Y |  | 參考值 |  |  |
| ref\_device\_id | varchar\(256\) | Y |  | 參考測點的設備識別名 |  |  |
| ref\_tag\_name | varchar\(128\) | Y |  | 參考測點名稱 |  |  |
| sample\_interval | integer | Y |  | 取樣間隔 |  |  |
| sample\_unit | varchar\(36\) | Y |  | 取樣間隔單位 |  |  |
| sample\_amount | integer | Y |  | 事件之後紀錄之取樣數量 |  | 值如果為0，代表「持續記錄」 |

### [API](#event-log-api) {#event-log-api}

* **create**

  * createEventLog
    * 包含紀錄測點一起帶給這支api
    * 紀錄測點為array, 可以為空
      * 空的話就只記錄本身\(事件測點\)的值

* **read**

  * getEventLog
    * 得到所有的事件紀錄，會附加每個event的詳細資訊
  * getRecordTagByEvent
    * 得到事件紀錄下的紀錄測點
  * getLogByEvent
    * 對應投影片裡的查詢表單設計的api

* **update**

  * updateEventLog
    * 更新事件紀錄的設定細節

* **delete**

  * deleteEventLogByName
    * 據事件紀錄名稱來刪除事件
  * deleteRecordTagByEvent
    * 刪除某事件底下的所有紀錄測點

### Question

##### archived Question

* 選擇參考測點時，是限定同一scada裡的tag嗎

  * yes

* 和益開會時好像有在設定畫面裡拿掉幾個欄位?

  * 遲滯範圍

* 有需要做到編輯event log的參考測點嗎?

  * 要，新增跟刪除

* 事件之後紀錄之取樣數量&持續記錄，是可以指定取樣數量且同時選擇持續記錄嗎?

  * 不行，只能二選一
  * 如果取樣數量設3
    * 連續觸發event的話，會取樣3個區間，直到重新觸發event後才會再取樣3次
  * 選擇持續記錄
    * 只要持續觸發event，就依照取樣間隔持續log

* 事件類型的種類有幾種?

  * 2類共4種

    * 類型

      * 事件測點跟參考測點相比

      * 事件測點跟參考值相比

* event\_sample\_amount和event\_sample\_keep\_log都設not null

  * 但這兩個是二選一的關係，所以用程式去控制

* 我想說 event\_log\_list 的primary key 你覺得 加上scadaId如何?不然都不能取重複名稱XD

  * ok, 把eventname和scada\_id都設pk

* 新  
  增事件紀錄時，是要所有資訊都放再同一表單一同用一支api來傳，還是先建立事件紀錄，再新增紀錄測點呢?

  * 同一支api一起建就好

##### in progress

### Note

* 如何利用上述schema將判斷是要用參考值或是參考測點?
  * 用event\_type來做
* event\_type的值限定1/2/3/4

### TODO

* scada-dbmanager
* api interface
* flyway

### 



