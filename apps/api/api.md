# API

---
## 1. Overview



### 3.3 Tag
#### 3.3.1 Models
##### 3.3.1.1 Tag {#model_Tag}
- Data Source: tag_list
- Properties: 

|Name|Data Type|Primary Key|Column Name|Description|
|:--:|:-------:|:-:|:--------:|:---------:|
|ScadaId|String|V|scada_id||
|DeviceId|String|V|device_id||
|TagName|String|V|tag_name||
|TagType|Number||tagtype||
|TagDesc|String||tag_description||
|ArraySize|Number||array_size||
|DataLog|Boolean||data_log||
|ReadOnly|Boolean||read_only|||

#### 3.3.2 Functions
##### 3.3.2.1 getTagsListWithScadaAndDevice {#function_getTagsListWithScadaAndDevice}
- Purpose: 列出所有特定 scadaId 和 deviceId 下的所有 Tags
- Input: 

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|deviceId|String|V||Device Id|
|count|Integer||1000|Data retrived. limit: 1000|
|index|Integer||1|Starting Index|
|tagName|String|||Filter Tag Name|
|tagDesc|String|||Filter Tag Description|
|tagType|Integer|||Filter Tag Type|
|sortby|String|||Sort by the specified property|
|order|String||DESC|ascending (ASC) or descending (DESC) only|
- Output:
 - 200 [TagCount](#model_TagCount) Object
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
 
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 利用 [formatRegexpFilter](#function_formatRegexpFilter) 產生 filter.where
 3. 檢查 index 和 count
 4. 利用 [formatSortBy](#function_formatSortBy) 產生 filter.order
 5. 呼叫 [countList](#function_countList) query Tag model with Scada Id and Device Id

##### 3.3.2.2 getTagsListWithScada {#function_getTagsListWithScada}
- Purpose: 列出所有特定 scadaId 下的所有 Tags

- Input: 

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|count|Integer||1000|Data retrived. limit: 1000|
|index|Integer||1|Starting Index|
|tagName|String|||Filter Tag Name|
|tagDesc|String|||Filter Tag Description|
|tagType|Integer|||Filter Tag Type|
|sortby|String|||Sort by the specified property|
|order|String||DESC|ascending (ASC) or descending (DESC) only|
- Output:
 - 200 [TagCount](#model_TagCount) Object
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
 
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 利用 [formatRegexpFilter](#function_formatRegexpFilter) 產生 filter.where
 3. 檢查 index 和 count
 4. 利用 [formatSortBy](#function_formatSortBy) 產生 filter.order
 5. 呼叫 [countList](#function_countList) query Tag model with ScadaId

##### 3.3.2.3 getTagsList {#function_getTagsList}
- Purpose: 列出所有 Tags

- Input: 

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|count|Integer||1000|Data retrived. limit: 1000|
|index|Integer||1|Starting Index|
|tagName|String|||Filter Tag Name|
|tagDesc|String|||Filter Tag Description|
|tagType|Integer|||Filter Tag Type|
|sortby|String|||Sort by the specified property|
|order|String||DESC|ascending (ASC) or descending (DESC) only|
- Output:
 - 200 [TagCount](#model_TagCount) Object
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
 
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 利用 [formatRegexpFilter](#function_formatRegexpFilter) 產生 filter.where
 3. 檢查 index 和 count
 4. 利用 [formatSortBy](#function_formatSortBy) 產生 filter.order
 5. 呼叫 [countList](#function_countList) query Tag model

##### 3.3.2.4 listAllTagsName {#function_listAllTagsName}
- Purpose: 列出特定 scadaId 和 deviceId 中所有 Tag name
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|deviceId|String|V||Device Id|
|count|Number|||Data retrived|
|index|Number|||Starting index|
- Output: 
 - 200 Array of [TagList](#model_TagList)
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 3. 檢查 index 和 count
 5. 呼叫 [countList](#function_countList) query Tag mode with ScadaId and DeviceId

##### 3.3.2.5 listTagByTagId {#function_listTagByTagId}
- Purpose: 列出特定 Tag Name 所有information
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|deviceId|String|V||Device Id|
|tagName|String|V||Tag name|
- Output: 
 - 200 Array of [TagList](#model_TagList)
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 呼叫 TagDao.getTag
 3. 呼叫 [_formatTagInfo](#function__formatTagInfo) 整理 output

##### 3.3.2.6 updateTag {#function_updateTag}
##### 3.3.2.7 _formatUpdateContent {#function__formatUpdateContent}
##### 3.3.2.8 _startUpdateTransaction {#function__startUpdateTransaction}
##### 3.3.2.9 _addModifiedConfigRecord {#function__addModifiedConfigRecord}
##### 3.3.2.10 _formatTagInfo {#function__formatTagInfo}
- Purpose: 整理Tag output
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|obj|Object|V||||
- Output: Object
- Logical description:
 1. 對應 obj 的 key 為 Tag 的properties

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


