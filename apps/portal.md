# Portal

---

## Table of Contents

#### 1. Overview

#### 2. Front-end architecture description

```
2.1   登入頁面  
  2.1.1   Login.vue

2.2   Device Management 頁面       
  2.2.1   ProjectList.vue
  2.2.2   ScadaList.vue
  2.2.3   DeviceList.vue
  2.2.4   TagList.vue

2.3   Alarm Summary 頁面
  2.3.1   AlarmSummary.vue

2.4   Account
  2.4.1   Account Setting 頁面
    2.4.1.1   AccountSetting.vue
  2.4.2   Role Setting 頁面
    2.4.2.1   RoleSetting.vue

2.5   WISE-PaaS Dashboard 頁面
  2.5.1   MenuBar.vue

2.6   User Guide 頁面
  2.6.1   UserGuide.vue

2.7   API Document 頁面 
 2.7.1   MenuBar.vue

2.8   使用者帳號管理
  2.8.1   Profile 頁面
    2.8.1.1   UserInfo.vue
  2.8.2   logout
```

--

### 1. Overview

### 2. Front-end architecture description

##### 2.1 登入頁面

* 2.1.1 Login.vue    
  * 目的:使用者輸入帳號密碼以登入WebAccess/SCADA  
  * 架構說明:  
    Vue-components  
    　　src/components/Login.vue  
    Vue-router  
    　　src/router/index.js
  * UI:
    ![](/assets/login.PNG)  
  * 邏輯說明:  
    1. Login Page  
       　1.1 進入Url: /Login。  
       　　1.1.1 透過index.js 進入Login.vue  
       　1.2 執行mounted function，檢查$cookie.get\("EIName"\)狀態，使用者是否登錄過。  
       　　1.2.1 若為true，執行checkToken function。  
       　　　1.2.1.1 呼叫$http.get\("/Users/me"\) 進入Device Management頁面。  
       　　1.2.2 若為false，則等待使用者按下1號按鈕。    
    2. 將帳號密碼輸入完後按下①按鈕，執行login function，檢查Conf.ValidSSO && Conf.mode === 'development'狀態。  
       　2.1 若為false，定義user這個objecct 並進入Device Management頁面。  
       　2.2 若為true，檢查帳號密碼是否已輸入。  
       　　2.2.1 呼叫axios.post\(Conf.SSOUrl + "/auth", obj\)檢查response.data.status === 'locked'狀態，使用者是否第一次  

     　　　　　　 註冊。  
    　　　2.2.1.1 若為true，addVisible=true   
    　　　2.2.1.2 若為false，檢查rememberMe狀態，2號按鈕是否被打勾，並且執行checkToken function。  
    　　　　2.2.1.2.1 若為true，記下帳號密碼，呼叫$http.get\("/Users/me"\) 進入 Device Management 頁面。   
    　　    　2.2.1.2.2 若為false，直接呼叫$http.get\("/Users/me"\) 進入 Device Management 頁面。   



