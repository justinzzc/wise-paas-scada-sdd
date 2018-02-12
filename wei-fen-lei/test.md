# Event log開發文件

### DB Schema

* event\_log\_ref \(參考測點\)

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar\(128\) | Y | Y | 事件紀錄名稱 | Y |
| ref\_scada\_id | varchar\(36\) | Y |  | 參考測點的節點識別碼 |  |
| ref\_device\_id | varchar\(256\) | Y |  | 參考測點的設備識別名 |  |
| ref\_tag\_name | varchar\(128\) | Y |  | 參考測點名稱 |  |

* event\_log\_record \(紀錄測點\)

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar\(128\) | Y | Y | 事件紀錄名稱 | Y |
| record\_scada\_id | varchar\(36\) | Y | Y | 紀錄測點的節點識別碼 | Y |
| record\_device\_id | varchar\(256\) | Y | Y | 紀錄測點的設備識別名 | Y |
| record\_tag\_name | varchar\(128\) | Y | Y | 紀錄測點名稱 | Y |

* event\_log\_list

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar\(128\) | Y | Y | 事件紀錄名稱 | Y |
| event\_description | varchar\(256\) | N |  | 事件描述 |  |
| scada\_id | varchar\(36\) | Y |  | 事件測點的節點識別碼 |  |
| device\_id | varchar\(256\) | Y |  | 事件測點的設備識別名 |  |
| tag\_name | varchar\(128\) | Y |  | 事件測點名稱 |  |
| event\_type | varchar\(128\) | Y |  | 事件類型 | Y |
| event\_ref\_value | double | Y |  | 參考值 |  |
| event\_sample\_interval | integer | Y |  | 取樣間隔 |  |
| event\_sample\_unit | varchar\(36\) | Y |  | 取樣間隔單位 | Y |
| event\_sample\_amount | integer | Y |  | 事件之後紀錄之取樣數量 |  |
| event\_sample\_keep\_log | boolean | Y |  | 持續記錄 |  |

* event\_type\_option

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| event\_type | varchar\(128\) | Y | Y | 事件類型 | Y |
| type\_description | varchar\(256\) | Y |  | 事件類型描述 \(顯示於UI\) |  |

* event\_unit\_option

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| event\_sample\_unit | varchar\(36\) | Y | Y | 取樣間隔單位 | Y |
| unit\_description | varchar\(36\) | Y |  | 取樣間隔單位描述\(顯示於UI\) |  |

### API

* Set
  * createEvent
  * addRefTag
  * deleteRefTag
* Get
  * getEvents
  * \_getRefTagCountOfEvent
  * getEventbyScada

### Question

##### archived Question

* 選擇參考測點時，是限定同一scada裡的tag嗎
  * yes

* 和益開會時好像有在設定畫面裡拿掉幾個欄位? 
  * 遲滯範圍
* 有需要做到編輯event log的參考測點嗎?
  * 要，新增跟刪除
* 事
  件之後紀錄之取樣數量&持續記錄，是可以指定取樣數量且同時選擇持續記錄嗎?
  * 不行，只能二選一
  * 如果取樣數量設3
    * 連續觸發event的話，會取樣3個區間，直到重新觸發event後才會再取樣3次
  * 選擇持續記錄
    * 只要持續觸發event，就依照取樣間隔持續log

##### in progress

* 要如何增加參考測點 \(UI\)

  * 待討論

* 我想說 event\_log\_list 的primary key 你覺得 加上scadaId如何?不然都不能取重複名稱XD

* 事  件類型的種類有幾種?

  * 2類共4種

  * 類型

    * 事件測點跟參考測點相比

    * 事件測點跟參考值相比

### Note

* Portal
  * 設定
    * API
    * UI
  * 查詢
    * API
    * UI



