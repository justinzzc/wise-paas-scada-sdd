
---

---

# Database

---

目前正式的資料表總計16個 \(之後配合會新增1個,預計17個表格\)

| No. | Table Name | Table Description |
| :--- | :--- | :--- |
| 1 | project\_list | 專案列表 |
| 2 | scada\_list | 節點列表 |
| 3 | device\_list | 設備列表 |
| 4 | tag\_list | 測點列表 |
| 5 | tag\_analog | 類比點資訊 |
| 6 | tag\_discrete | 離散點資訊 |
| 7 | tag\_text | 文字點資訊 |
| 8 | alarm\_analog | 類比點警報資訊 |
| 9 | alarm\_discrete | 離散點警報資訊 |
| 10 | role | 角色列表 |
| 11 | scope | 權限列表 |
| 12 | scope\_role | 權限-角色關係 |
| 13 | user\_info | 帳戶資訊 |
| 14 | user\_scope | 帳戶-權限關係 |
| 15 | user\_allow\_device | 設備儀器列表 |
| 16 | sys\_parameters | 系統參數 |
| 17 | scada\_parameters | 節點參數 |

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
| alarm\_status | boolean | Y |  | 是否有警報屬性 |  |
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

* 


