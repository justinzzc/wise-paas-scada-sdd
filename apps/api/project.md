## 3. Component-Level Design

### Project
#### Models
##### Project {#model_Project}
|     name    | id |     type    | allowNull |      validation      |
|:-----------:|:--:|:-----------:|:---------:|:--------------------:|
|  projectId  |  v |  STRING(32) |   false   | Length, special char |
| description |    | STRING(256) |    true   |        Length        |

##### ProjectCount {#model_ProjectCount}
|    name    | id |  type  |            Description           |
|:----------:|:--:|:------:|:--------------------------------:|
|    list    |  v |  Array | [Project](#model_Project) Object |
| totalCount |    | Number |          Total Instance          |
|    index   |    | Number |          Starting Index          |
|    count   |    | Number |           Data retrived          |

##### ProjectUpdateInstance {#model_ProjectUpdateInstance}
|     name    | id |     type    | allowNull |      validation      |
|:-----------:|:--:|:-----------:|:---------:|:--------------------:|
|  projectId  |  v |  STRING(32) |    true   | Length, special char |
| description |    | STRING(256) |    true   |        Length        |

#### Functions
#####
#### API
|                HTTP Request               |          URL         | Authorization |               Description              |
|:-----------------------------------------:|:--------------------:|:-------------:|:--------------------------------------:|
|         [POST](#api_POST/Projects)        |       /Projects      |  Edit_Config  |          Create a new project          |
|          [GET](#api_GET/Projects)         |       /Projects      |               |            List all projects           |
|    [GET](#api_GET/Projects/:projectId)    | /Projects/:projectId |               | List the specified project information |
|    [PUT](#api_PUT/Projects/:projectId]    | /Projects/:projectId |  Edit_Config  |       Update an existing project       |
| (DELETE](#api_DELETE/Projects/:projectId] | /Projects/:projectId |  Edit_Config  |      Delete the specified project      |

##### [`POST /Projects`] {#api_POST/Projects}
##### [`GET /Projects`] {#api_GET/Projects}
##### [`GET /Projects/:projectId`] {#api_GET/Projects/:projectId}
##### [`PUT /Projects/:projectId`] {#api_PUT/Projects/:projectId}
##### [`DELETE /Projects/:projectId`] {#api_DELETE/Projects/:projectId}