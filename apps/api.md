# API

---
## 1. Overview

## 2. Director Tree
```
│
├ common
│   ├ models
│   │   ├ const.js
│   │   ├ device.js
│   │   ├ device.json
│   │   ├ hist-data.js
│   │   ├ hist-data.json 
│   │   ├ project.js
│   │   ├ project.json
│   │   ├ real-data.js
│   │   ├ real-data.json
│   │   ├ role-info.js
│   │   ├ role-info.json
│   │   ├ scada.js
│   │   ├ scada.json
│   │   ├ scope-info.js
│   │   ├ scope-info.json
│   │   ├ status.js
│   │   ├ status.json
│   │   ├ tag.js
│   │   ├ tag.json
│   │   ├ token.js
│   │   ├ token.json
│   │   ├ user-info.js
│   │   └ user-info.json
│   └ utils
│        ├ db.js
│        ├ error.js
│        └ utils.js
├ scripts
│   ├ gen_man.bat
│   └ gen_yml.bat
├ server
│   ├ explorer
│   │   ├ bwclient.ico
│   │   ├ index.html
│   │   ├ index.yml
│   │   ├ swagger-ui-bundle.js
│   │   ├ swagger-ui-bundle.js.map
│   │   ├ swagger-ui-standalone-preset.js
│   │   ├ swagger-ui-standalone-preset.js.map
│   │   ├ swagger-ui.css
│   │   ├ swagger-ui.css.map
│   │   ├ swagger-ui.js
│   │   └ swagger-ui.js.map
│   ├ models
│   │   ├ analog-tag.js
│   │   ├ analog-tag.json
│   │   ├ count.js
│   │   ├ count.json
│   │   ├ device-count.js
│   │   ├ device-count.json
│   │   ├ device-list.js
│   │   ├ device-list.json
│   │   ├ device-update-instance.js
│   │   ├ device-update-instance.json
│   │   ├ discrete-tag.js
│   │   ├ discrete-tag.json
│   │   ├ hist-data-log.js
│   │   ├ hist-data-log.json
│   │   ├ hist-raw-data.js
│   │   ├ hist-raw-data.json
│   │   ├ role-insert-instance.js
│   │   ├ role-insert-instance.json
│   │   ├ role-scope.js	
│   │   ├ role-scope.json
│   │   ├ role-update-instance.js
│   │   ├ role-update-instance.json
│   │   ├ scada-count.js
│   │   ├ scada-count.json
│   │   ├ scada-instance.js
│   │   ├ scada-instance.json
│   │   ├ scada-list.js
│   │   ├ scada-list.json
│   │   ├ scada-update-instance.js
│   │   ├ scada-update-instance.json
│   │   ├ sync-res.js
│   │   ├ sync-res.json
│   │   ├ sys-param.js
│   │   ├ sys-param.json
│   │   ├ tag-count.js
│   │   ├ tag-count.json
│   │   ├ tag-list.js
│   │   ├ tag-list.json
│   │   ├ tag-req.js
│   │   ├ tag-req.json
│   │   ├ tag-update-instance.js
│   │   ├ tag-update-instance.json
│   │   ├ tag-value.js
│   │   ├ tag-value.json
│   │   ├ text-tag.js
│   │   ├ text-tag.json	
│   │   ├ user-count.js
│   │   ├ user-count.json
│   │   ├ user-insert-instance.js
│   │   ├ user-insert-instance.json
│   │   ├ user-instance.js
│   │   ├ user-instance.json
│   │   ├ user-scope.js
│   │   ├ user-scope.json
│   │   ├ user-update-instance.js
│   │   ├ user-update-instance.json
│   │   ├ value.js
│   │   └ value.json
│   ├ component-config.json
│   ├ config.json
│   ├ datasources.development.json
│   ├ datasources.json
│   ├ datasources.production.js
│   ├ middleware.development.json
│   ├ middleware.json
│   ├ model-config.json
│   └ server.js
├ test
│   ├ deviceTest.js
│   ├ realdataTest.js
│   ├ roleTest.js
│   ├ scadaTest.js
│   ├ tagTest.js
│   └ userTest.js
├ .babelrc
├ .cfignore
├ .gitignore
├ CHANGELOG.md
├ Procfile
├ README.md
├ config.json
├ gulpfile.js
└ package.json
```
## 3. Component-Level Design
### 3.1 SCADA
#### 3.1.1 Models
##### 3.1.1.1 Scada {#model_Scada}
- Data Source: scada_list
- Properties: 

|Name|Data Type|Primary Key|Column Name|Description|
|:--:|:-------:|:-:|:--------:|:---------:|
|ScadaId|String|V|scada_id||
|ScadaType|Number||scada_type||
|ScadaName|String||scada_name||
|ScadaDesc|String||scada_description||
|PrimaryScadaIP|String||primary_scada_ip||
|BackupScadaIP|String||backup_scada_ip||
|PrimaryScadaPort|Number||primary_scada_port||
|BackupScadaPort|Number||backup_scada_port|||

##### 3.1.1.2 ScadaCount {#model_ScadaCount}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|List|Array|[SCADA](#model_Scada) Object|
|TotalCount|Number|The total count instances of the model from the data source matched by where filter|
|Index|Number|Starting Index|
|Count|Number|Data retrived|

##### 3.1.1.3 ScadaInstance {#model_ScadaInstance}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|ScadaId|String|scada_id|

##### 3.1.1.3 ScadaList {#model_ScadaList}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|ScadaId|String|scada_id|
|ScadaName|String|scada_name|

##### 3.1.1.4 ScadaUpdateInstance {#model_ScadaUpdateInstance}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|ScadaDesc|String|scada_description|
- Validation:
 - **scadaDesc**: length <= 256

#### 3.1.2 Functions
##### 3.1.2.1 listAllScada
- Purpose: List all SCADA information
- Input: 

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|count|Integer||1000|Data retrived. limit: 1000|
|index|Integer||1|Starting Index|
|scadaName|String|||Filter Scada Name|
|scadaDesc|String|||Filter Scada Description|
|sortby|String|||Sort by the specified property|
|order|String||DESC|ascending (ASC) or descending (DESC) only|
- Output:
 - 200 [ScadaCount](#model_ScadaCount) Object
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
 
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 利用 [formatRegexpFilter](#function_formatRegexpFilter) 產生 filter.where
 3. 檢查 index 和 count
 4. 利用 [formatSortBy](#function_formatSortBy) 產生 filter.order
 5. 呼叫 [countList](#function_countList) query SCADA model

##### 3.1.2.2 listAllScadaName
- Purpose: 列出所有 scada Id 和 Name
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
- Output:
 - 200 Array of [ScadaList](#model_ScadaList) Object
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error

- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. Query Scada model ScadaId 和 ScadaName 欄位

##### 3.1.2.3 listScadaById
- Purpose: 列出特定scadaId的SCADA所有資訊
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
- Output:
 - 200 [Scada](#model_Scada) Object
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. Query Scada model by ScadaId

##### 3.1.2.4 updateScada
- Purpose: 更新特定scadaId的SCADA資訊
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|obj|[ScadaUpdataInstance](#model_ScadaUpdataInstance)|V||object of update properties|
- Output: 
 - 200 Boolean. return true, if update successfully.
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 檢查 [ScadaUpdataInstance](#model_ScadaUpdateInstance) 是否合法
 3. 將可更新 properties 整理 update obj
 4. 檢查 ScadaId 是否存在
 5. 呼叫 [_startUpdateTransaction](#function__startUpdateTransaction) 更新scada

##### 3.1.2.5 syncScada
- Purpose: 同步特定scadaId的SCADAs資訊至webAccess
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|where|Object|V||request Object|
|data|Array|V||Arrary of [ScadaInstance](#model_ScadaInstance)|
- Output:
- 200 Array of SyncRes(#model_SyncRes)
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 檢查並整理傳給 deviceManager 的 array of scadaId
 3. 呼叫 deviceManager.syncDeviceConfig
 4. 整理回傳結果 (res) 成 Arrary of [ScadaInstance](#model_ScadaInstance)
   - output.ScadaId = res.id
   - output.IsSuccess = res.ok
   - output.ErrMsg = res.message

##### 3.1.2.6 _startUpdateTransaction #{function__startUpdateTransaction}
- Purpose: 使用 transaction 處理 update scada 和 deviceManager
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|where|Object|V||Filter object|
|scada|Object|V||Update SCADA|
|cb|Function|V||Callback function|
- Output:
 - 200 Boolean. return true, if update successfully.
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 整理傳給 deviceManager 的 params
   - params[ScadaId] = scada
 2. 開啟 transaction
 3. 更新符合 where 條件的 Scada

### 3.2 Device
#### 3.2.1 Models
##### 3.2.1.1 Device {#model_Device}
- Data Source: device_list
- Properties: 

|Name|Data Type|Primary Key|Column Name|Description|
|:--:|:-------:|:-:|:--------:|:---------:|
|ScadaId|String|V|scada_id||
|DeviceId|String|V|device_id||
|DeviceName|String||device_name||
|DeviceType|Number||devcie_type||
|DeviceIP|String||device_ip||
|DeviceDesc|String||device_description||
|ComportNbr|Number||comport_nbr|||

#### 3.2.2 Functions
### 3.3 Tag
### 3.4 HistData
### 3.5 RealData
### 3.6 Status
### 3.7 Token
#### 3.7.1 Models
#### 3.7.2 Functions
##### 3.7.2.1 refreshToken
##### 3.7.2.1 validToken{#function_validToken}
##### 3.7.2.1 validScope
##### 3.7.2.1 getToken
##### 3.7.2.1 _validToken
##### 3.7.2.1 _canAccessSCADASrp
### 3.8 UserInfo
### 3.9 RoleInfo
### 3.10 ScopeInfo
### 3.11 SysParam
### 3.12 Count
#### 3.12.1 Models
##### 3.12.1.1 Count{#model_count}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|List|Array|Object|
|TotalCount|Number|The total count instances of the model from the data source matched by where filter|
|Index|Number|Starting Index|
|Count|Number|Data retrived|

#### 3.12.2 Functions
##### 3.12.2.1 countList {#function_countList}
- Purpose: Query the specify model instances using the input filter and format the output
- Input:

|Name|Data Type|Description|
|:--:|:-------:|:---------:|
|Model|Object|Loopback model|
|filter|Object|Loopback filter object|
|index|Number|Starting Index|
|count|Number|Data retrived|
|cb|Function|Callback function|

- Output:
 - 200 [Count](#model_count) Object
 - 404 Result not found
 - 500 Internal error
- Logical description:
 1. 計算 Model 中符合 filter 條件的 instance 數量(totalCount)
 2. Query model using the filter(objs)
 3. 整理 output [Count](#model_count) Object
   - output.ToltalCount = totalCount
   - output.List = objs
   - output.Count = objs.length
   - output.Index = index

### 3.13 Utils
#### 3.13.1 Functions
##### 3.13.1.1 isValueExist
##### 3.13.1.1 disableAllMethodsBut
##### 3.13.1.1 isObject
##### 3.13.1.1 hasValue
##### 3.13.1.1 checkIPFormat
##### 3.13.1.1 isEmpty
##### 3.13.1.1 isEmptylbObj
##### 3.13.1.1 isValueInObject
##### 3.13.1.1 isInt
##### 3.13.1.1 formatRegexpFilter{#function_formatRegexpFilter}
##### 3.13.1.1 isOrder
##### 3.13.1.1 formatCountResponse
##### 3.13.1.1 formatSortBy{#function_formatSortBy}
##### 3.13.1.1 formatWhereFilter
##### 3.13.1.1 isKeyInlbObj
### 3.14 SyncRes
#### 3.14.1 Models
##### 3.14.1.1 SyncRes {#model_SyncRes}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|ScadaId|String|Scada Id|
|IsSuccess|Boolean|If synchronized successfully or not|
|ErrMsg|String|Error message|


