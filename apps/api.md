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
|scadaId|String|V|scada_id||
|scadaType|Number||scada_type||
|scadaName|String||scada_name||
|scadaDesc|String||scada_description||
|primaryScadaIP|String||primary_scada_ip||
|backupScadaIP|String||backup_scada_ip||
|PrimaryScadaPort|Number||primary_scada_port||
|backupScadaPort|Number||backup_scada_port||

##### 3.1.1.2 ScadaCount {#model_ScadaCount}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|list|Array|[SCADA](#model_Scada) Object|
|totalCount|Number||
|index|Number||
|count|Number||

##### 3.1.1.3 ScadaInstance {#model_ScadaInstance}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|scadaId|String|scada_id|

##### 3.1.1.3 ScadaList {#model_ScadaList}
- Properties:

|Name|Data Type|Description|
|:--:|:-------:|:-:|:--------:|
|scadaId|String|scada_id|
|scadaName|String|scada_name|

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
 5. 呼叫 [countList](#function_countList) query SCADA model.

##### 3.1.2.2 listAllScadaName
- Purpose: 
- Input: request object
- Output:
- Logical description:


##### 3.1.2.3 listScadaById
- Purpose: 
- Input:
- Output:
- Logical description:


##### 3.1.2.4 updateScada
- Purpose: 
- Input:
- Output:
- Logical description:


##### 3.1.2.5 syncScada
- Purpose: 
- Input:
- Output:
- Logical description:


##### 3.1.2.6 _startUpdateTransaction
- Purpose: 
- Input:
- Output:
- Logical description:



### 3.2 Device
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
|list|Array|Object|
|totalCount|Number|The total count instances of the model from the data source matched by where filter|
|index|Number|Starting Index|
|count|Number|Data retrived|

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
   - output.toltalCount = totalCount
   - output.list = objs
   - output.count = objs.length
   - output.index = index

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

