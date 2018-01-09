# API

---
## 1. Overview



## 3.4 HistData
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


