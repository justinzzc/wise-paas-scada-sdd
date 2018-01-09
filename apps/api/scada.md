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
|:--:|:-------:|:--------:|
|List|Array|[SCADA](#model_Scada) Object|
|TotalCount|Number|The total count instances of the model from the data source matched by where filter|
|Index|Number|Starting Index|
|Count|Number|Data retrived|

##### 3.1.1.3 ScadaInstance {#model_ScadaInstance}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:--------:|
|ScadaId|String|scada_id|

##### 3.1.1.3 ScadaList {#model_ScadaList}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:--------:|
|ScadaId|String|scada_id|
|ScadaName|String|scada_name|

##### 3.1.1.4 ScadaUpdateInstance {#model_ScadaUpdateInstance}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:--------:|
|ScadaDesc|String|scada_description|
- Validation:
 - **ScadaDesc**: length <= 256

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
 - 200 Array of [SyncRes](#model_SyncRes)
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
 3. 更新符合 where 條件的 Scada 並呼叫deviceManager.addModifiecConfigRecord