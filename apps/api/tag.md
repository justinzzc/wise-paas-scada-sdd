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