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

##### 3.7.2.1 validToken {#function_validToken}

##### 3.7.2.1 validScope

##### 3.7.2.1 getToken

##### 3.7.2.1 \_validToken

##### 3.7.2.1 \_canAccessSCADASrp

### 3.8 UserInfo

### 3.9 RoleInfo

### 3.10 ScopeInfo

### 3.11 SysParam

### 3.12 Count

#### 3.12.1 Models

##### 3.12.1.1 Count {#model_count}

* Properties:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| List | Array | Object |
| TotalCount | Number | The total count instances of the model from the data source matched by where filter |
| Index | Number | Starting Index |
| Count | Number | Data retrived |

#### 3.12.2 Functions

##### 3.12.2.1 countList {#function_countList}

* Purpose: Query the specify model instances using the input filter and format the output
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| Model | Object | Loopback model |
| filter | Object | Loopback filter object |
| index | Number | Starting Index |
| count | Number | Data retrived |
| cb | Function | Callback function |

* Output:
  * 200 [Count](#model_count) Object
  * 404 Result not found
  * 500 Internal error
* Logical description:
  1. 計算 Model 中符合 filter 條件的 instance 數量\(totalCount\)
  2. Query model using the filter\(objs\)
  3. 整理 output [Count](#model_count) Object
     * output.ToltalCount = totalCount
     * output.List = objs
     * output.Count = objs.length
     * output.Index = index

### 3.13 Utils

#### 3.13.1 Functions

##### 3.13.1.1 isValueExist \(val\)

* 判斷輸入值是否為`undefined`, `null`, `''` ，若是則回傳false。
* return: boolean

##### 3.13.1.1 disableAllMethodsBut \(model, exposeMethods\)

* \[NEED REVIEW\] 將loopback model提供的預設remote method全部關閉，例如deleteById, updateAttributes等等，只允許自訂的remoteMethod。
* 詳情請參考[https://github.com/strongloop/loopback/issues/651](https://github.com/strongloop/loopback/issues/651 "Disable all remote methods and enable only selected ones \(white-list\) \#651")

##### 3.13.1.1 isObject \(obj\)

* `return (obj && obj.constructor && obj.constructor === Object);`

##### 3.13.1.1 hasValue \(obj, value\)

* 尋找輸入的物間是否包含輸入值
* return: boolean

##### 3.13.1.1 checkIPFormat \(ip\)

* 檢查IPV4的格式是否正確。
* return: boolean

##### 3.13.1.1 isEmpty \(obj\)

* 檢查物件是否為空，利用null/length/typeof/hasOwnProperty來判斷。
* return: boolean

##### 3.13.1.1 isEmptylbObj \(lbObj\)

* 判斷傳入的loopback物件是否為空
* return: boolean

##### 3.13.1.1 isValueInObject \(obj, value\)

* 判斷輸入值是否存在於輸入的物件
* return: boolean
* 討論: 
  * 是不是跟hasValue重複?
  * 有需要加上hasOwnProperty嗎?

##### 3.13.1.1 isInt \(n\)

* 判斷是否為Int
* return: boolean

##### 3.13.1.1 formatRegexpFilter {#function_formatRegexpFilter}

##### 3.13.1.1 isOrder \(str\)

* 判斷字串中是否包含ASC或DESC

##### 3.13.1.1 formatCountResponse

##### 3.13.1.1 formatSortBy {#function_formatSortBy}

##### 3.13.1.1 formatWhereFilter

##### 3.13.1.1 isKeyInlbObj \(lbObj, key\)

* 判斷loopback物件中是否有包含key name
* return: boolean

##### 3.13.1.1 hasSpecial \(str\)

* 判斷字串裡是否包含特殊字元
* return: boolean

##### 3.13.1.1 getUserNameByToken \(token\)

* 將SSO Token透過jwt.decode取得userName，若是測試且繞過SSO驗證則直接使用const.js裡定義的email

### 3.14 SyncRes

#### 3.14.1 Models

##### 3.14.1.1 SyncRes {#model_SyncRes}

* Properties:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| ScadaId | String | Scada Id |
| IsSuccess | Boolean | If synchronized successfully or not |
| ErrMsg | String | Error message |



