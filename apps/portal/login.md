# 登入頁面

---

#####  登入頁面
- 2.1.1 Login.vue    
  - 目的:使用者輸入帳號密碼以登入WebAccess/SCADA  
  - 架構說明:  
  Vue-components  
  　　src/components/Login.vue  
  Vue-router  
  　　src/router/index.js
  - UI:
   ![](/assets/login.PNG)  
  - 邏輯說明:  
        1. Login Page  
        　1.1 進入Url: /Login。  
        　　1.1.1 透過index.js 進入Login.vue  
        　1.2 執行mounted function，檢查$cookie.get("EIName")狀態，使用者是否登錄過。  
        　　1.2.1 若為true，執行checkToken function。  
        　　　1.2.1.1 呼叫$http.get("/Users/me") 進入Device Management頁面。  
        　　1.2.2 若為false，則等待使用者按下1號按鈕。  
        2.將帳號密碼輸入完後按下①按鈕，執行login function，檢查Conf.ValidSSO && Conf.mode === 'development'狀態。  
        　2.1 若為false，定義user這個objecct 並進入Device Management頁面。  
        　2.2 若為true，檢查帳號密碼是否已輸入。  
        　　2.2.1 呼叫axios.post(Conf.SSOUrl + "/auth", obj)檢查response.data.status === 'locked'狀態，使用者是否第一次註冊。  
        　　　2.2.1.1 若為true，addVisible=true   
        　　　2.2.1.2 若為false，檢查rememberMe狀態，2號按鈕是否被打勾，並且執行checkToken function。  
        　　　　2.2.1.2.1 若為true，記下帳號密碼，呼叫$http.get("/Users/me") 進入 Device Management 頁面。  
        　　　　2.2.1.2.2 若為false，直接呼叫$http.get("/Users/me") 進入 Device Management 頁面。   




