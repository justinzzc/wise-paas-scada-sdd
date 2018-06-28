# Database

---

目前正式的資料表總計17個 \(之後配合會新增2個\(Alarm\),預計19個表格\)

| No. | Table Name | Table Description |
| :--- | :--- | :--- |
| 1 | project\_list | 專案列表 |
| 2 | scada\_list | 節點列表 |
| 3 | device\_list | 設備列表 |
| 4 | tag\_list | 測點列表 |
| 5 | tag\_analog | 類比點資訊 |
| 6 | tag\_discrete | 離散點資訊 |
| 7 | tag\_text | 文字點資訊 |
| 8 | role | 角色列表 |
| 9 | scope | 權限列表 |
| 10 | scope\_role | 權限-角色關係 |
| 11 | user\_info | 帳戶資訊 |
| 12 | user\_scope | 帳戶-權限關係 |
| 13 | user\_allow\_device | 設備儀器列表 |
| 14 | sys\_parameters | 系統參數 |
| 15 | scada\_parameters | 節點參數 |
| 16 | event\_log\_list | 事件設定值、事件測點和參考測點 |
| 17 | event\_log\_record | 紀錄測點 |
| 18 | alarm\_list | 警報設定 |
| 19 | alarm\_tag | 警報點 |

* project\_list

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| proj\_id | varchar\(32\) | Y | Y | 專案識別名 | Y |
| proj\_description | varchar\(256\) | N |  | 專案敘述 |  |

* scada\_list

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id | varchar\(36\) | Y | Y | 節點識別碼 | Y |
| scada\_name | varchar\(128\) | Y |  | 節點名稱 |  |
| scada\_description | varchar\(256\) | N |  | 節點敘述 |  |
| primary\_scada\_ip | varchar\(32\) | N |  | 主要節點IP |  |
| primary\_scada\_port | integer | N |  | 主要節點通訊埠 |  |
| backup\_scada\_ip | varchar\(32\) | N |  | 次要節點IP |  |
| backup\_scada\_port | integer | N |  | 次要節點通訊埠 |  |
| scada\_type | integer | Y |  | 節點類型 |  |
| heartbeat | integer | Y |  | 頻率 |  |
| proj\_id | varchar\(32\) | N |  | 專案識別名 |  |
| config\_uploaded | boolean | Y |  | 是否已上傳 |  |

* device\_list

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id | varchar\(36\) | Y | Y | 節點識別碼 | Y |
| device\_id | varchar\(256\) | Y | Y | 設備識別名 | Y |
| device\_name | varchar\(128\) | Y |  | 設備名稱 |  |
| comport\_nbr | integer | N |  | 設備通訊埠 |  |
| device\_description | varchar\(256\) | N |  | 設備敘述 |  |
| device\_ip | varchar\(32\) | N |  | 設備IP |  |
| device\_port | integer | N |  | 設備通訊埠 |  |
| device\_type | varchar\(32\) | Y |  | 設備類型 |  |

* tag\_list

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id | varchar\(36\) | Y | Y | 節點識別碼 | Y |
| device\_id | varchar\(256\) | Y | Y | 設備識別名 | Y |
| tag\_name | varchar\(128\) | Y | Y | 測點名稱 | Y |
| tag\_description | varchar\(256\) | N |  | 測點敘述 |  |
| tag\_type | integer | Y |  | 測點類型 |  |
| array\_size | integer | Y |  | 陣列大小 |  |
| data\_log | boolean | Y |  | 資料紀錄 |  |
| read\_only | boolean | Y |  | 唯獨 |  |

* tag\_analog

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id | varchar\(36\) | Y | Y | 節點識別碼 | Y |
| device\_id | varchar\(256\) | Y | Y | 設備識別名 | Y |
| tag\_name | varchar\(128\) | Y | Y | 測點名稱 | Y |
| eng\_unit | varchar\(256\) | Y |  | 工程單位 |  |
| span\_high | double | Y |  | 最高量程 |  |
| span\_low | double | Y |  | 最低量程 |  |
| int\_dsp\_fmt | integer | Y |  | 整數位數 |  |
| fra\_dsp\_fmt | integer | Y |  | 小數點位數 |  |

* tag\_discrete

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id | varchar\(36\) | Y | Y | 節點識別碼 | Y |
| device\_id | varchar\(256\) | Y | Y | 設備識別名 | Y |
| tag\_name | varchar\(128\) | Y | Y | 測點名稱 | Y |
| state\_0 | varchar\(32\) | Y |  | 狀態0 |  |
| state\_1 | varchar\(32\) | Y |  | 狀態1 |  |
| state\_2 | varchar\(32\) | Y |  | 狀態2 |  |
| state\_3 | varchar\(32\) | Y |  | 狀態3 |  |
| state\_4 | varchar\(32\) | Y |  | 狀態4 |  |
| state\_5 | varchar\(32\) | Y |  | 狀態5 |  |
| state\_6 | varchar\(32\) | Y |  | 狀態6 |  |
| state\_7 | varchar\(32\) | Y |  | 狀態7 |  |

* tag\_text

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id | varchar\(36\) | Y | Y | 節點識別碼 | Y |
| device\_id | varchar\(256\) | Y | Y | 設備識別名 | Y |
| tag\_name | varchar\(128\) | Y | Y | 測點名稱 | Y |

* role

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| role\_id | varchar\(32\) | Y | Y | 角色識別名 |  |
| role\_description | varchar\(256\) | N |  | 角色敘述 |  |

* scope

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scope\_id | varchar\(32\) | Y | Y | 權限識別名 |  |
| scope\_description | varchar\(256\) | N |  | 權限敘述 |  |

* scope\_role

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scope\_id | varchar\(32\) | Y | Y | 權限識別名 |  |
| role\_id | varchar\(32\) | Y | Y | 角色識別名 |  |

* scope\_role

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| user\_id | integer | Y | Y | 使用者ID\(自動編號\) | Y |
| user\_name | varchar\(128\) | Y |  | 使用者名稱 |  |
| email | varchar\(256\) | N |  | 使用者E-mail |  |
| sso\_role | varchar\(32\) | N |  | SSO角色 |  |
| user\_description | varchar\(256\) | N |  | 使用者敘述 |  |
| create\_user | integer | N |  | 建立人員 |  |

* user\_scope

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| user\_id | integer | Y | Y | 權限識別名 | Y |
| scope\_id | varchar\(32\) | Y | Y | 角色識別名 |  |

* user\_allow\_device

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| user\_id | integer | Y | Y | 使用者ID\(自動編號\) | Y |
| proj\_id | varchar\(32\) | Y | Y | 使用者名稱 | Y |
| scada\_id | varchar\(36\) | N | Y | 使用者E-mail | Y |
| device\_id | varchar\(256\) | N | Y | SSO角色 | Y |

* sys\_parameters

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| param\_name | varchar\(32\) | Y | Y | 參數名稱 | Y |
| param\_value | varchar\(256\) | Y | Y | 參數值 |  |

* scada\_parameters

| Column Name | Type | Not Null | PK | Description | Index |
| :--- | :--- | :--- | :--- | :--- | :--- |
| scada\_id | varchar\(36\) | Y | Y | 參數名稱 | Y |
| param\_name | varchar\(32\) | Y | Y | 參數值 |  |
| param\_value | varchar\(256\) | N |  |  |  |

* event\_log\_list

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

* event\_log\_record

| Column Name | Type | Not Null | PK | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| event\_id | integer | Y | Y |  | Y |  |
| device\_id | varchar\(256\) | Y | Y | 紀錄測點的設備識別名 | Y |  |
| tag\_name | varchar\(128\) | Y | Y | 紀錄測點名稱 | Y |  |

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

* E-R Diagram

\(1\) Device Management:

設備管理主要分為四層:專案層-&gt;節點層-&gt;設備層-&gt;測點層

一個專案底下可以有多個節點\(Project: SCADA=1:m\)

一個節點底下可以有多個設備\(SCADA: Device=1:m\)

一個設備底下可以有多個測點\(Device: Tag=1:m\)

一個測點底下必須有一個測點類型細項\(Tag: Analog/Discrete/Text=1:1\)

一個"警報類比"/"警報離散"測點底下有一個測點警報細項\(Tag: Analog/Discrete/Text=1:1\)

![](/assets/deviceMangement.png)

\(2\) Account Setting

一個專案底下可以有多個節點\(Project: SCADA=1:m\)

一個專案底下可以有多個設備\(Project: Device=1:m\)

一個帳戶底下可以有多個專案\(User: Project=1:m\)

一個帳戶底下可以有多個設備允許觀看的權限\(User: Allow\_Device=1:m\)

一個帳戶可以多個權限組合不可重複\(User: Scope=1:m\)

一個帳戶與權限組合不可重複

![](/assets/accseeting.png)

\(3\) Role Setting

![](/assets/roleseeting.png)

