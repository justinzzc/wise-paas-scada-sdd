# Event log開發文件

### DB Schema

* event\_log\_ref

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| event\_name | varchar |  |  |  |  |
| scada\_id | varchar |  |  |  |  |
| device\_id |  |  |  |  |  |
| tag\_name |  |  |  |  |  |

* event\_log\_list

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id |  |  |  |  |  |
| event\_name |  |  |  |  |  |
| event\_description |  |  |  |  |  |
| event\_tag |  |  |  |  |  |
| event\_type |  |  |  |  |  |
| event\_ref\_value |  |  |  |  |  |
| event\_sample\_interval |  |  |  |  |  |
| event\_sample\_unit |  |  |  |  |  |
| event\_sample\_amount |  |  |  |  |  |
| event\_sample\_keep\_log |  |  |  |  |  |

* event\_type\_option

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| type\_id | varchar |  |  |  |  |
| type\_description | varchar |  |  |  |  |

* event\_unit\_option

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| unit\_id |  |  |  |  |  |
| unit\_description |  |  |  |  |  |

### API

* Set
  * createEvent
  * addRefTag
  * deleteRefTag
* Get
  * getEvents
  * getRefTagCountOfEvent
  * getEventbyScada

### Question

* 選擇參考測點時，是限定同一scada裡的tag嗎
* 事件類型的種類有幾種?應該是只有兩種? \(&gt;=和&lt;=\)
* 和益開會時好像有在設定畫面裡拿掉幾個欄位? ex. 遲滯範圍
* 有需要做到編輯event log的參考測點嗎?
* 要如何增加參考測點 \(UI\)?
* 事件之後紀錄之取樣數量&持續記錄，不太懂
  * 是可以指定取樣數量且同時選擇持續記錄嗎?

### Note

* Portal
  * 設定
    * API
    * UI
  * 查詢
    * API
    * UI



