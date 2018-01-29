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

\| Column Name \| Type  
 \| Not  Null \| PK \| Description \| Index \|  
\| :--- \| :--- \| :--- \| :--- \| :--- \| :--- \|  
\| scada\_id \| varchar\(36\) \| Y \| Y \| 專案識別名 \| Y \|  
\| scada\_name \| varchar\(128\) \| Y \|  \|  \|  \|  
\| scada\_description \| varchar\(256\) \| N \|  \|  \|  \|  
\| primary\_scada\_ip  \| varchar\(32\) \| N \|  \|  \|  \|  
\| primary\_scada\_port  \|  integer  \| N \|  \|  \|  \|  
\| backup\_scada\_ip \| varchar\(32\) \| N \|  \|  \|  \|  
\| backup\_scada\_port \|  integer  \| N \|  \|  \|  \|  
\| scada\_type  \|  integer  \| N \|  \|  \|  \|  
\| heartbeat  \|  integer  \| N \|  \|  \|  \|  
\| proj\_id \| varchar\(32\) \| N \|  \|  \|  \|  
\| config\_uploaded \| boolean \| N \|  \|  \|  \|

* 


