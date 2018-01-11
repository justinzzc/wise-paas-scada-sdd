## 3. Component-Level Design

### Project
#### 1. Models
##### 1.1 Project {#model_Project}
|     Name    | Id |     Type    | allowNull |      Validation      |
|:-----------:|:--:|:-----------:|:---------:|:--------------------:|
|  projectId  |  v |  String(32) |   false   | Length, special char |
| description |    | String(256) |    true   |        Length        |

##### 1.2 ProjectCount {#model_ProjectCount}
|    Name    | Id |  Type  |            Description           |
|:----------:|:--:|:------:|:--------------------------------:|
|    list    |  v |  Array | [Project](#model_Project) Object |
| totalCount |    | Number |          Total Instance          |
|    index   |    | Number |          Starting Index          |
|    count   |    | Number |           Data retrived          |

##### 1.3 ProjectUpdateInstance {#model_ProjectUpdateInstance}
|     Name    | Id |     Type    | allowNull |      Validation      |
|:-----------:|:--:|:-----------:|:---------:|:--------------------:|
|  projectId  |  v |  String(32) |    true   | Length, special char |
| description |    | String(256) |    true   |        Length        |

#### 2. Functions
##### 2.1 createProject {#function_createProject}
###### Description: check input and create Project
###### input
|    Name    |   Type   | Default |            Note           |
|:----------:|:--------:|:-------:|:-------------------------:|
|     req    |  Object  |         |                           |
| projectObj |  Object  |         | [Project](#model_Project) |
|      cb    | Function |         |     callback function     |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 檢查 [projectObj](#model_Project) 是否合法
3. 呼叫`UserDao.getUserByUserName`取得 userId
4. 開啟 Transaction
5. 呼叫`ProjectDao.insertProject(projectObj, trans)` insert Project
6. 呼叫`UserAllowDeviceDao.insertAccessRight([userId, projectId], trans)` insert Project 存取權限
7. 如果 Transaction 成功, 回傳true, 否則rollback並回傳錯誤

##### 2.2 listAllProjects {#function_listAllProjects}
###### Description: check input and get project list
###### input
|     Name    |   Type   |  Default  |        Note       |
|:-----------:|:--------:|:---------:|:-----------------:|
|     req     |  Object  |           |                   |
|    count    |  Number  |    1000   |    Limit 1000    |
|    index    |  Number  |     1     |                   |
|  projectId  |  String  |    null   |                   |
| description |  String  |    null   |                   |
|    sortby   |  String  | projectId |                   |
|     asc     |  Boolean |    true   |                   |
|      cb     | Function |           | callback function |

###### Logical
1. 檢查 Token, 並取得 userName
2. 檢查 input 是否皆合法
3. 整理 filter Object
    - filter.offset = index - 1
    - filter.limit = count
    - filter.projectId = projectId
    - filter.description = description
    - filter.sortby = sortby
    - filter.order = asc
    - filter.detail = true
    - filter.userName = userName
4. 呼叫`ProjectDao.getProjectList(filter)`, 取得response(res)
5. 整理 output, 並回傳output
    - output.list = res.rows
    - output.Count = res.count
    - output.index = index
    - output.count = res.rows.length

##### 2.3 listProjectById {#function_listProjectById}
###### Description: check input and get the specified project 
###### input
|    Name   |   Type   | Default |        Note       | 
|:---------:|:--------:|:-------:|:-----------------:|
|    req    |  Object  |         |                   |
| projectId |  String  |         |                   |
|     cb    | Function |         | callback function |

###### Logical
1. 檢查 Token, 並取得 userName
2. 呼叫`ProjectDao.checkProjectRightByUserName(userName)` 檢查 user 是否有存取 projectId 的權限
3. 呼叫`ProjectDao.getProject(projectId)`取得 project 的資訊, 並回傳 response

##### 2.4 updateProject {#function_updateProject}
###### Description: check input and update the specified project
###### input
|     Name   |   Type   | Default |                          Note                         | 
|:----------:|:--------:|:-------:|:-----------------------------------------------------:|
|     req    |  Object  |         |                                                       |
|  projectId |  String  |         |                                                       |
| projectObj |  Object  |    {}   | [ProjectUpdateInstance](#model_ProjectUpdateInstance) |
|      cb    | Function |         |                   callback function                   |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 檢查 projectObj 是否為空
3. 檢查 [projectObj](#model_ProjectUpdateInstance) 是否合法
4. 整理 obj, 擷取可以Project更新的properties(#model_ProjectUpdateInstance)
5. 呼叫`ProjectDao.checkProjectRightByUserName(userName)` 檢查 user 是否有存取 projectId 的權限, 及 projectId 是否存在
6. 開啟 Transaction
7. 呼叫`ProjectDao.updateProject(projectId, obj, trans)` update Project
8. 如果 Transaction 成功, 回傳true, 否則rollback並回傳錯誤

##### 2.5 deleteProject {#function_deleteProject}
###### Description: check input and delete the specified project, and the SCADAs and Devices in the project
###### input
|    Name   |   Type   | Default |        Note       | 
|:---------:|:--------:|:-------:|:-----------------:|
|    req    |  Object  |         |                   |
| projectId |  String  |         |                   |
|     cb    | Function |         | callback function |

###### Logical
1. 檢查 Token 和 user scope, 並取得 userName
2. 呼叫`ProjectDao.checkProjectRightByUserName(userName)` 檢查 user 是否有存取 projectId 的權限
3. 呼叫`ScadaDao.checkScadaRightByUserName(userName)`和`DeviceDao.checkDeviceRightByUserName(userName)`檢查 user 是否有存取 projectId 下所有 scada 和 device的權限
4. 開啟 Transaction
5. 如果 scada configUploaded 是 true, 則呼叫`scadaManager.addModifiedConfigRecord(scadaId, {scadaId: null})`, 通知 scadaManager 刪除 scada, 並將 scadaId 加入 scadaIds 中
6. 如果 scada configUploaded 是 false, 則將 scadaId 加入 deleteScadas 中
7. 呼叫`scadaManager.syncScadaConfig(scadaIds)`同步 config, 並接受同步結果 (result)
8. 如果 result 中 scadaId 同步成功, 則加入 deleteScadas 中
9. 呼叫`ScadaDao.deleteScada(deleteScadas, trans)`刪除 scadas 及其下所有 devices
10. 呼叫`ProjectDao.deleteProject(projectId, trans)`刪除 project 及其下所有 scada 和 device
11. 如果 Transaction 失敗, 則rollback並回傳錯誤, 如果成功則整理 output
    - output 為每個 scada 的 config [同步狀況](otherModel.md#model_SyncRes)
    - 若 project 下沒有 scada 則回傳空 Array
    - 若 scada 不需要同步, 則 isSuccess = true

#### 3. API
|         HTTP Request         |          URL         | Authorization |               Description              |                   Function                   |
|:----------------------------:|:--------------------:|:-------------:|:--------------------------------------:|:--------------------------------------------:|
|  [POST](#api_insertProject)  |       /Projects      |  Edit_Config  |          Create a new project          |   [createProject](#function_createProject)   |
|   [GET](#api_listProjects)   |       /Projects      |               |            List all projects           | [listAllProjects](#function_listAllProjects) |
|    [GET](#api_getProject)    | /Projects/:projectId |               | List the specified project information | [listProjectById](#function_listProjectById) |
|   [PUT](#api_updateProject)  | /Projects/:projectId |  Edit_Config  |       Update an existing project       |   [updateProject](#function_updateProject)   |
| [DELETE](#api_deleteProject) | /Projects/:projectId |  Edit_Config  |      Delete the specified project      |   [deleteProject](#function_deleteProject)   |

##### 3.1 `POST /Projects` {#api_insertProject}
###### Description: Create a new project 
###### Input
| Name |               Type               | Necessary | Default |     Description     |
|:----:|:--------------------------------:|:---------:|:-------:|:-------------------:|
|  req |              Object              |     v     |         | Http Request Object |
| data | [Project](#model_Project) Object |     v     |         |   Project Instance  |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Boolean   |                                        |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     500     | Error Object |             Internal Error             |

##### 3.2 `GET /Projects` {#api_listProjects}
###### Description: List all projects
###### Input
|     Name    |   Type  | Necessary |  Default  |           Description          |
|:-----------:|:-------:|:---------:|:---------:|:------------------------------:|
|     req     |  Object |     v     |           |       Http Request Object      |
|    count    |  Number |           |    1000   |   Data retrived(Limit 10000)   |
|    index    |  Number |           |     1     |         Starting Index         |
|  projectId  |  String |           |           |      Filter by project Id      |
| description |  String |           |           |  Filter by project description |
|    sortby   |  String |           | projectId | Sort by the specified property |
|     asc     | Boolean |           |    true   |       order by asc or not      |

###### Output
| Status Code |     Type     |                 Description                |
|:-----------:|:------------:|:------------------------------------------:|
|     200     |    Object    | [ProjectCount](#model_ProjectCount) Object |
|     400     | Error Object |                Input Invalid               |
|     401     | Error Object |   No Authorization or Token Format Error   |
|     403     | Error Object |              Permission Denied             |
|     500     | Error Object |               Internal Error               |

##### 3.3 `GET /Projects/:projectId` {#api_getProject}
###### Description: List the specified project information
###### Input
|    Name   |  Type  | Necessary | Default |     Description     |
|:---------:|:------:|:---------:|:-------:|:-------------------:|
|    req    | Object |     v     |         | Http Request Object |
| projectId | String |     v     |         |      Project Id     |

###### Output
| Status Code |     Type     |               Description              |
|:-----------:|:------------:|:--------------------------------------:|
|     200     |    Object    |    [Project](#model_Project) Object    |
|     400     | Error Object |              Input Invalid             |
|     401     | Error Object | No Authorization or Token Format Error |
|     403     | Error Object |            Permission Denied           |
|     404     | Error Object |            Result Not Found            |
|     500     | Error Object |             Internal Error             |

##### 3.4 `PUT /Projects/:projectId` {#api_updateProject}
###### Description: Update an existing project
###### Input
|    Name   |  Type  | Necessary | Default |                          Description                         |
|:---------:|:------:|:---------:|:-------:|:------------------------------------------------------------:|
|    req    | Object |     v     |         |                      Http Request Object                     |
| projectId | String |     v     |         |                          Project Id                          |
|    data   | Object |     v     |         | [ProjectUpdateInstance](#model_ProjectUpdateInstance) Object |

###### Output
| Status Code |     Type     |                Description               |
|:-----------:|:------------:|:----------------------------------------:|
|     200     |    Boolean   | If updated sucessfully, then return true |
|     400     | Error Object |               Input Invalid              |
|     401     | Error Object |  No Authorization or Token Format Error  |
|     403     | Error Object |             Permission Denied            |
|     404     | Error Object |             Result Not Found             |
|     500     | Error Object |              Internal Error              |

##### 3.5 `DELETE /Projects/:projectId` {#api_deleteProject}
###### Delete the specified project
###### Input
|    Name   |  Type  | Necessary | Default |                          Description                         |
|:---------:|:------:|:---------:|:-------:|:------------------------------------------------------------:|
|    req    | Object |     v     |         |                      Http Request Object                     |
| projectId | String |     v     |         |                          Project Id                          |

###### Output
| Status Code |     Type     |                                      Description                                      |
|:-----------:|:------------:|:-------------------------------------------------------------------------------------:|
|     200     |     Array    | [SyncRes](otherModel.md#model_SyncRes) Object <br> Return each Scada status, which is in ProjectId |
|     400     | Error Object |                                     Input Invalid                                     |
|     401     | Error Object |                         No Authorization or Token Format Error                        |
|     403     | Error Object |                                   Permission Denied                                   |
|     404     | Error Object |                                    Result Not Found                                   |
|     500     | Error Object |                                     Internal Error                                    |