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
|    list    |  v |  Array | [Project](#Model_Project) Object |
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
##### `POST /Projects`
##### `GET /Projects`
##### `GET /Projects/:projectId`
##### `PUT /Projects/:projectId`
##### `DELETE /Projects/:projectId`