### SCADA
#### 1. Models
##### 1.1 Scada {#model_Scada}
|       Name       | Id |     Type    |
|:----------------:|:--:|:-----------:|
|     projectId    |    |  String(32) |
|      scadaId     |  v |  String(36) |
|     scadaType    |    |    Number   |
|     scadaName    |    | String(128) |
|    description   |    | String(256) |
|  primaryScadaIP  |    |  String(32) |
|   backupScadaIP  |    |  String(32) |
| primaryScadaPort |    |    Number   |
|  backupScadaPort |    |    Number   |
|     heartbeat    |    |    Number   |
|  configUploaded  |    |   Boolean   |

##### 1.2 ScadaCount {#model_ScadaCount}
|    Name    | Id |  Type  |          Description         |
|:----------:|:--:|:------:|:----------------------------:|
|    list    |  v |  Array | [Scada](#model_Scada) Obejct |
| totalCount |    | Number |        Total Instance        |
|    index   |    | Number |        Starting Index        |
|    count   |    | Number |         Data retrived        |

##### 1.3 ScadaInsertInstance {#model_ScadaInsertInstance}
|     Name    | Id |     Type    | allowNull |      Validation      |
|:-----------:|:--:|:-----------:|:---------:|:--------------------:|
|  projectId  |  v |  String(32) |   false   | Length, special char |
|  scadaName  |    | String(128) |   false   | Length, special char |
| description |    | String(256) |    true   |        Length        |

##### 1.4 ScadaInstance {#model_ScadaInstance}
|   Name  | Id |    Type    |
|:-------:|:--:|:----------:|
| scadaId |  v | String(36) |

##### 1.5 ScadaList {#model_ScadaList}
|    Name   | Id |     Type    |
|:---------:|:--:|:-----------:|
| projectId |    |  String(32) |
|  scadaId  |  v |  String(36) |
| scadaName |    | String(128) |

##### 1.6 ScadaStatus {#model_ScadaStatus}
|   Name   | Id |    Type    |          Description         |
|:--------:|:--:|:----------:|:----------------------------:|
|  scadaId |  v | String(36) |                              |
|  status  |    |   Boolean  |                              |
| modified |    |   Boolean  | If config is modified or not |

##### 1.7 ScadaUpdateInstance {#model_ScadaUpdateInstance}
|     Name    | Id |     Type    | allowNull | Validation |
|:-----------:|:--:|:-----------:|:---------:|:----------:|
| description |  v | String(256) |    true   |   Length   |

##### 1.8 BindScadaInstance {#model_BindScadaInstance}
|    Name   | Id |    Type    |
|:---------:|:--:|:----------:|
| projectId |  v | String(32) |
|  scadaId  |  v | String(36) |

#### 2 Functions
##### 2.1 createScada {#function_createScada}
###### Description: Create a new scada
###### input
|   Name   |   Type   | Default |        Note       |
|:--------:|:--------:|:-------:|:-----------------:|
|    req   |  Object  |         |                   |
| scadaObj |  Object  |    {}   |                   |
|    cb    | Function |         | callback function |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 檢查 [scadaObj](#model_ScadaInsertInstance) 是否合法
3. 產生UUID, scadaObj.scadaId = UUID
4. 呼叫`ProjectDao.checkProjectRightByUserName(userName)` 檢查 user 是否有存取 projectId 的權限
5. 呼叫`ScadaDao.getScada(scadaId)`檢查 scadaId 是否重複
6. 開啟 Transaction
7. 呼叫`ScadaDao.insertScada(scadaObj, trans)` insert Scada
8. 呼叫`UserAllowDeviceDao.insertAccessRight([{userId, projectId, scadaId}], trans)` 增加 userId 存取 scadaId 的權限
9. 呼叫`scadaManager.upsertScadaStatus(scadaId, {})` 更新 scada 狀態
10. 如果 Transaction 成功, 則回傳建立的 scada object, 否則 rollback 並回傳 Error

##### 2.2 listAllScada {#function_listAllScada}
###### Description: List all SCADA information
###### input
|     Name    |   Type   | Default |        Note       |
|:-----------:|:--------:|:-------:|:-----------------:|
|     req     |  Object  |         |                   |
|    count    |  Number  |   1000  |     Limit 1000    |
|    index    |  Number  |    1    |                   |
|  scadaName  |  String  |   null  |                   |
| description |  String  |   null  |                   |
|    sortby   |  String  | scadaId |                   |
|     asc     |  Boolean |   true  |                   |
|      cb     | Function |         | callback function |

###### Logical
1. 檢查 Token, 並取得 userName
2. 檢查 input 是否皆合法
3. 整理 filter Object
    - filter.offset = index - 1
    - filter.limit = count
    - filter.scadaName = scadaName
    - filter.description = description
    - filter.sortby = sortby
    - filter.order = asc
    - filter.detail = true
    - filter.userName = userName
4. 呼叫`ScadaDao.getScadaList(filter)`, 取得response(res)
5. 整理 output, 並回傳output
    - output.list = res.rows
    - output.Count = res.count
    - output.index = index
    - output.count = res.rows.length

##### 2.3 listAllScadaName {#function_listAllScadaName}
###### Description: List all Project Id, SCADA Id and SCADA name
###### input
| Name |   Type   | Default |        Note       |
|:----:|:--------:|:-------:|:-----------------:|
|  req |  Object  |         |                   |
|  cb  | Function |         | callback function |

###### Logical
1. 檢查 Token, 並取得 userName
2. 整理 filter Object
    - filter.detail = false
    - filter.userName = userName
3. 呼叫`ScadaDao.getScadaList(filter)`, 取得response(res)
4. 整理 output, 並回傳output
    - output.list = res.rows
    - output.Count = res.count
    - output.index = 1
    - output.count = res.rows.length

##### 2.4 listScadaByProjectId {#function_listScadaByProjectId}
###### Description: List SCADA information in the specified project
###### input
|     Name    |   Type   | Default |        Note       |
|:-----------:|:--------:|:-------:|:-----------------:|
|     req     |  Object  |         |                   |
|  projectId  |  String  |         |                   |
|    count    |  Number  |   1000  |     Limit 1000    |
|    index    |  Number  |    1    |                   |
|  scadaName  |  String  |   null  |                   |
| description |  String  |   null  |                   |
|    sortby   |  String  | scadaId |                   |
|     asc     |  Boolean |   true  |                   |
|      cb     | Function |         | callback function |

###### Logical
1. 檢查 Token, 並取得 userName
2. 檢查 input 是否皆合法
3. 整理 filter Object
    - filter.offset = index - 1
    - filter.limit = count
    - filter.scadaName = scadaName
    - filter.description = description
    - filter.sortby = sortby
    - filter.order = asc
    - filter.detail = true
    - filter.userName = userName
4. 呼叫`ProjectDao.checkProjectRightByUserName(userName)`檢查 userId 是否有存取 projectId 權限
5. 呼叫`ScadaDao.getScadaListByProjectId(projectId, filter)`, 取得response(res)
6. 整理 output, 並回傳output
    - output.list = res.rows
    - output.Count = res.count
    - output.index = index
    - output.count = res.rows.length

##### 2.5 getScada {#function_getScada}
###### Description: Get the specified SCADA information
###### input
|    Name   |   Type   | Default |        Note       |
|:---------:|:--------:|:-------:|:-----------------:|
|    req    |  Object  |         |                   |
| projectId |  String  |         |                   |
|  scadaId  |  String  |         |                   |
|     cb    | Function |         | callback function |

###### Logical
1. 檢查 Token, 並取得 userName
2. 呼叫`ScadaDao.checkScadaRightByUserName(userName)`檢查 userId 是否有權限存取 scadaId
3. 呼叫`ScadaDao.getScada(scadaId)`取得 scadaId 資訊並回傳

##### 2.6 updateScada {#function_updateScada}
###### Description: Update the specified SCADA information
###### input
|   Name   |   Type   | Default |                           Note                           |
|:--------:|:--------:|:-------:|:--------------------------------------------------------:|
|    req   |  Object  |         |                                                          |
|  scadaId |  String  |         |                                                          |
| scadaObj |  Object  |    {}   | [ScadaUpdateInstance](#model_ScadaUpdateInstance) Object |
|    cb    | Function |         |                     callback function                    |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 檢查 [scadaObj](#model_ScadaUpdateInstance) 是否合法
3. 擷取可以 update 的 properties 並整理 update object(_scada)
4. 呼叫`ScadaDao.checkScadaRightByUserName(userName)`檢查 userId 是否有權限存取 scadaId, 並檢查 scadaId 是否存在
5. 開啟 Transaction
6. 呼叫`ScadaDao.updateScada(scadaId, _scada, trans)`更新 scada
7. 如果 scadaId 沒有 upload config 過, 則return true
8. 如果 scadaId 有 upload config 過, 則呼叫`scadaManager.addModifiedConfigRecord(scadaId, params)`同步 config
9. 如果同步成功, 則回傳 true, 否則 rollback 並回傳 Error 和失敗原因

##### 2.7 bindScada {#function_bindScada}
###### Description: Bind existing SCADAs to project
###### input
|    Name   |   Type   | Default |                         Note                         |
|:---------:|:--------:|:-------:|:----------------------------------------------------:|
|    req    |  Object  |         |                                                      |
| instances |   Array  |    []   | [BindScadaInstance](#model_BindScadaInstance) Object |
|     cb    | Function |         |                   callback function                  |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 呼叫`ProjectDao.checkProjectRightByUserName(userName)`檢查 userId 是否對全部新的 projectId 有存取權限
3. 呼叫`ScadaDao.checkScadaRightByUserName(userName)`檢查 userId 是否對全部 scadaId 有存取權限
4. 開啟 Transaction
5. 呼叫`ScadaDao.bindScadas(instances, trans)`重新綁定 bind 到新的 project 上
6. 如果成功, 則回傳 true, 否則 rollback 並回傳錯誤

##### 2.8 deleteScada {#function_deleteScada}
###### Description: Delete the specified SCADA information
###### input
|   Name  |   Type   | Default |        Note       |
|:-------:|:--------:|:-------:|:-----------------:|
|   req   |  Object  |         |                   |
| scadaId |  String  |         |                   |
|    cb   | Function |         | callback function |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 呼叫`ProjectDao.checkProjectRightByUserName(userName)`檢查 userId 是否對全部新的 projectId 有存取權限
3. 呼叫`ScadaDao.checkScadaRightByUserName(userName)`檢查 userId 是否對全部 scadaId 有存取權限
4. 開啟 Transaction
5. 呼叫`ScadaDao.bindScadas(instances, trans)`重新綁定 bind 到新的 project 上
6. 如果成功, 則回傳 true, 否則 rollback 並回傳錯誤

##### 2.9 syncScada {#function_syncScada}
###### Description: Synchronize the specified SCADA information
###### input
|   Name   |   Type   | Default |                     Note                     |
|:--------:|:--------:|:-------:|:--------------------------------------------:|
|    req   |  Object  |         |                                              |
| scadaIds |   Array  |    []   | [ScadaInstance](#model_ScadaInstance) Object |
|    cb    | Function |         |               callback function              |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 檢查 [scadaIds](#model_ScadaInstance) 是否合法
3. 擷取 scadaId (scadaIds)
4. 呼叫`ScadaDao.checkScadaRightByUserName(userName)`檢查 userId 是否有權限存取所有 scadaId, 並檢查 scadaId 是否存在
5. 呼叫`scadaManager.syncScadaConfig(scadaIds)`同步 scada config, 取得response(result)
6. 整理 output 為每個 scadaId 同步情況與錯誤訊息, 並回傳output
  - output[idx].scadaId = result[idx].id
  - output[idx].isSuccess = result[idx].ok
  - output[idx].errMsg = result[idx].message

#### 3. API
|           HTTP Request           |                URL               | Authorization |                   Description                   |                        Function                        |
|:--------------------------------:|:--------------------------------:|:-------------:|:-----------------------------------------------:|:------------------------------------------------------:|
|     [POST](#api_createScada)     |              /Scadas             |  Edit_Config  |                Create a new scada               |          [createScada](#function_createScada)          |
|   [GET](#api_listAllScadaName)   |           /Scadas/list           |               |   List all Project Id, SCADA Id and SCADA name  |     [listAllScadaName](#function_listAllScadaName)     |
|     [GET](#api_listAllScada)     |           /Scadas/info           |               |            List all SCADA information           |         [listAllScada](#funciton_listAllScada)         |
| [GET](#api_listScadaByProjectId) |      /Scadas/info/:projectId     |               | List SCADA information in the specified project | [listScadaByProjectId](#function_listScadaByProjectId) |
|       [GET](#api_getScada)       | /Scadas/info/:projectId/:scadaId |               |       Get the specified SCADA information       |             [getScada](#function_getScada)             |
|      [PUT](#api_updateScada)     |         /Scadas/:scadaId         |  Edit_Config  |      Update the specified SCADA information     |          [updateScada](#function_updateScada)          |
|    [DELETE](#api_deleteScada)    |         /Scadas/:scadaId         |  Edit_Config  |      Delete the specified SCADA information     |          [deleteScada](#function_deleteScada)          |
|      [POST](#api_syncScada)      |           /Scadas/sync           |  Edit_Config  |   Synchronize the specified SCADA information   |            [syncScada](#function_syncScada)            |
|      [POST](#api_bindScada)      |           /Scadad/bind           |  Edit_Config  |         Bind existing SCADAs to project         |            [bindScada](#function_bindScada)            |

##### 3.1 `POST /Scadas` {#api_createScada}
###### Description: Create a new scada 
###### Input
| Name |          Type         | Necessary | Default |     Description     |
|:----:|:---------------------:|:---------:|:-------:|:-------------------:|
|  req |         Object        |     v     |         | Http Request Object |
| data | [Scada](#model_Scada) |     v     |         |    SCADA instance   |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Object    |      [Scada](#model_Scada) Object      |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     500     | Error Object |             Internal Error             |

##### 3.2 `GET /Scadas/list` {#api_listAllScadaName}
###### Description: List all Project Id, SCADA Id and SCADA name
###### Input
| Name |          Type         | Necessary | Default |     Description     |
|:----:|:---------------------:|:---------:|:-------:|:-------------------:|
|  req |         Object        |     v     |         | Http Request Object |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |     Array    |  [ScadaList](#model_ScadaList) Object  |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     500     | Error Object |             Internal Error             |

##### 3.3 `GET /Scadas/info` {#api_listAllScada}
###### Description: List all SCADA information
###### Input
|     Name    |   Type  | Necessary | Default |           Description          |
|:-----------:|:-------:|:---------:|:-------:|:------------------------------:|
|     req     |  Object |     v     |         |       Http Request Object      |
|    count    |  Number |           |   1000  |   Data retrived. limit: 1000   |
|    index    |  Number |           |    1    |         Starting Index         |
|  scadaName  |  String |           |         |        Filter Scada Name       |
| description |  String |           |         |    Filter Scada Description    |
|    sortby   |  String |           | scadaId | Sort by the specified property |
|     asc     | Boolean |           |   true  |       Order by asc or not      |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Object    | [ScadaCount](#model_ScadaCount) Object |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     500     | Error Object |             Internal Error             |

##### 3.4 `GET /Scadas/info/:projectId` {#api_listScadaByProjectId}
###### Description: List SCADA information in the specified project
###### Input
|     Name    |    Type   | Necessary | Default |           Description          |
|:-----------:|:---------:|:---------:|:-------:|:------------------------------:|
|     req     |   Object  |     v     |         |       Http Request Object      |
|  projectId  | projectId |     v     |         |           Project Id           |
|    count    |   Number  |           |   1000  |   Data retrived. limit: 1000   |
|    index    |   Number  |           |    1    |         Starting Index         |
|  scadaName  |   String  |           |         |        Filter Scada Name       |
| description |   String  |           |         |    Filter Scada Description    |
|    sortby   |   String  |           | scadaId | Sort by the specified property |
|     asc     |  Boolean  |           |   true  |       Order by asc or not      |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Object    | [ScadaCount](#model_ScadaCount) Object |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     404     | Error Object |            Result Not Found            |
|     500     | Error Object |             Internal Error             |

##### 3.5 `GET /Scadas/info/:projectId/:scadaId` {#api_getScada}
###### Description: Get the specified SCADA information
###### Input
|    Name   |  Type  | Necessary | Default |     Description     |
|:---------:|:------:|:---------:|:-------:|:-------------------:|
|    req    | Object |     v     |         | Http Request Object |
| projectId | String |     v     |         |      Project Id     |
|  scadaId  | String |     v     |         |       SCADA Id      |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Object    |      [Scada](#model_Scada) Object      |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     404     | Error Object |            Result Not Found            |
|     500     | Error Object |             Internal Error             |

##### 3.6 `PUT /Scadas/:scadaId` {#api_updateScada}
###### Description: Update the specified SCADA information
###### Input
|   Name  |                        Type                       | Necessary | Default |      Description     |
|:-------:|:-------------------------------------------------:|:---------:|:-------:|:--------------------:|
|   req   |                       Object                      |     v     |         |  Http Request Object |
| scadaId |                       String                      |     v     |         |       SCADA Id       |
|   data  | [ScadaUpdateInstance](#model_ScadaUpdateInstance) |     v     |         | SCADA updated object |

###### Output
| Status Code |     Type     |                  Description                  |
|:-----------:|:------------:|:---------------------------------------------:|
|     200     |    Boolean   | If config is updated sucessfully, return true |
|     400     | Error Object |                 Input Invalid                 |
|     401     | Error Object |     No Authorization or Token Format Error    |
|     403     | Error Object |               Permission Denied               |
|     404     | Error Object |                Result Not Found               |
|     500     | Error Object |                 Internal Error                |

##### 3.7 `DELETE /Scadas/:scadaId` {#api_deleteScada}
###### Description: Delete the specified SCADA information
###### Input
|   Name  |  Type  | Necessary | Default |     Description     |
|:-------:|:------:|:---------:|:-------:|:-------------------:|
|   req   | Object |     v     |         | Http Request Object |
| scadaId | String |     v     |         |       SCADA Id      |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Object    |    [SyncRes](#model_SyncRes) Object    |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     404     | Error Object |            Result Not Found            |
|     500     | Error Object |             Internal Error             |

##### 3.8 `POST /Scadas/sync` {#api_syncScada}
###### Description: Synchronize the specified SCADA information
###### Input
| Name |                  Type                 | Necessary | Default |     Description     |
|:----:|:-------------------------------------:|:---------:|:-------:|:-------------------:|
|  req |                 Object                |     v     |         | Http Request Object |
| data | [ScadaInstance](#model_ScadaInstance) |     v     |         |       SCADA Id      |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |     Array    |    [SyncRes](#model_SyncRes) Object    |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     404     | Error Object |            Result Not Found            |
|     500     | Error Object |             Internal Error             |

##### 3.9 `POST /Scadad/bind` {#api_bindScada}
###### Description: Bind existing SCADAs to project
###### Input
| Name |                      Type                     | Necessary | Default |           Description           |
|:----:|:---------------------------------------------:|:---------:|:-------:|:-------------------------------:|
|  req |                     Object                    |     v     |         |       Http Request Object       |
| data | [BindScadaInstance](#model_BindScadaInstance) |     v     |         | New projectId and scadaId pairs |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Boolean   |   If bind is sucessfully, return true  |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     404     | Error Object |             Result Not Found           |
|     500     | Error Object |              Internal Error            |