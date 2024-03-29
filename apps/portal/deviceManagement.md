# Device Management頁面

---

- 1.ProjectList.vue  
  - 目的:使用者在Device Management 頁面可以編輯project的Id及敘述，並可新增新的project項目，另外也可以執行刪除動作。 
  - 架構說明:  
  Vue-components  
  　　src/components/ProjectManger/ProjectList.vue  
  　　src/components/MenuBar.vue  
  　　src/components/Common/Modal.vue  
  　　src/components/Common/ConfigModal.vue   
  Vue-router  
  　　src/router/index.js   
  - UI:  
  ![](/assets/projectlist.PNG)
  ![](/assets/project_edit.png)
  ![](/assets/project_delete.png)  
  ![](/assets/newproject.png)  
  - 邏輯說明:  
        1. 在Device Management中編輯Project的頁面(Url: /Login/CloudManager/DeviceManagement)  
        　1.1 透過src/components/MenuBar.vue按下1號按鈕執行select function，透過src/router/index.js呼叫src/components/ProjectList.vue進入可編輯Project的頁面。  
        　1.2 同時在src/components/MenuBar.vue檢查$store.state.user.scope使用者的權限將變數存入scopeEdit。  
        2. 一進入Project的頁面即執行$checkLogin和getProjectList function。  
        　2.1 執行$checkLogin判斷scopeEdit狀態。  
          　2.1.1 若為true即可顯示4、5號按鈕。  
          　2.1.2 若為false即不顯示4、5、7號按鈕。  
          2.2 執行getProjectList列出所有project相關資訊。  
        　　2.2.1 呼叫API: GET /Projects。  
        　　　2.2.1.1 若呼叫成功列出project資料，並將要顯示資訊存到projects陣列中。  
        　　　2.2.1.2 若呼叫失敗則判斷err.response.status === 404狀態。  
        　　　　2.2.1.2.1 若為true，令projects = []。  
        　　　　2.2.1.2.2 若為false，執行$checkToken。   
        3. 按下2號按鈕執行getProjectList並依使用者選擇或輸入做排序。  
        4. 按下3號按鈕執行handleDetailClick。  
        　4.1 呼叫API: /Projects/:projectId。  
        　　4.1.1 若呼叫成功，令detailVisible = true，並將要顯示的資訊存到tempProject物件中。  
        　　　4.1.1.1 呼叫src/components/Common/ConfigModal.vue，並傳入Config、typeEdit、obj、type等參數，分別為:Config="true" :typeEdit="scopeEdit" :obj="tempProject" :type="'project'"。  
        　　　　4.1.1.1.1 若呼叫成功則跳出detail視窗  
        　　　　　4.1.1.1.1.1 若typeEdit=true即顯示7號按鈕，按下7號按鈕執行handleEditClick。  
        　　　　　　4.1.1.1.1.1.1 分別將顯示的參數存入showObj物件，編輯參數存入editObj物件，並判斷obj.alarm!==undefined。  
        　　　　　　　4.1.1.1.1.1.1.1 若為true將obj.alarm參數存入showAlarm和editAlarm。  
        　　　　　4.1.1.1.1.2 編輯完後按下9號按鈕執行handleSaveClick。  
        　　　　　　4.1.1.1.1.2.1 呼叫API: PUT /Projects/{projectId}。  
        　　　　　　　4.1.1.1.1.2.1.1 若呼叫成功即完成編輯，並回到一開始detail畫面。  
        　　　　　　　4.1.1.1.1.2.1.2 若呼叫失敗回傳errMsg。  
        　　　　　4.1.1.1.1.3 若不想儲存編輯按下8號按鈕，令editObj = {}並回到一開始detail畫面。  
        　　　　　4.1.1.1.1.4 按下10號按鈕，令tempProject = {}並令detailVisible = False關閉視窗。  
        　　　4.1.2 若呼叫失敗執行$checkToken。  
        5. 按下4號按鈕執行handleDeleteClick。  
        　5.1 將選取的project資訊存入tempProject物件中，並令deleteVisiable = true。  
        　　5.1.1 呼叫src/components/Common/Modal.vue，顯示delete視窗。  
        　　　5.1.1.1 按下11號按鈕執行handleDeleteSubmit。  
        　　　　5.1.1.1.1 執行deleteProject並呼叫API: DELETE /Projects/{projectId}。  
        　　　　　5.1.1.1.1.1 若呼叫成功，將畫面上選取的project資訊刪除，並令deleteVisiable = false關閉視窗。  
        　　　　　5.1.1.1.1.2 若呼叫失敗，顯示errStr，並令deleteVisiable = false關閉視窗。  
        　　　5.1.1.2 按下12號按鈕執行handleCancelClick。  
        　　　　5.1.1.2.1 令tempProject = {}，並令deleteVisiable = false關閉視窗。  
        6. 按下5號按鈕執行handleAddClick。  
        　6.1 令addVisible = true和tempProject = {}。  
        　　6.1.1 呼叫src/components/Common/Modal.vue，顯示New Project視窗。  
        　　　6.1.1.1 按下13號按鈕，執行handleAddSubmitClick。  
        　　　　6.1.1.1.1 判斷tempProject.projectId狀態。  
        　　　　　6.1.1.1.1.1 若為flase，顯示errMsg。  
        　　　　　6.1.1.1.1.2 若為true，執行createNewProject。  
        　　　　　　6.1.1.1.1.2.1 呼叫API: POST /Projects。  
        　　　　　　　6.1.1.1.1.2.1.1 若呼叫成功執行getProjectList，並令addVisible = false關閉視窗。  
        　　　　　　　6.1.1.1.1.2.1.2 若呼叫失敗顯示errMsg。  
         　　　　6.1.1.2 按下14號按鈕，執行handleCancelClick。  
         　　　　　6.1.1.2.1 令tempProject = {}並令addVisible = false關閉視窗。  
        7. 按下6號按鈕執行handleProjectClick。  
        　7.1 透過src/router/index.js呼叫src/components/ScadaList.vue進入可編輯SCADA的頁面。     
  
  
  

          
        


- 2.ScadaList.vue  
  - 目的:使用者在Device Management 頁面可以編輯SCADA的Name、敘述、新增新的SCADA項目、同步、將SCADA移動至其他Project，另外也可以執行刪除動作。 
  - 架構說明:  
  Vue-components  
  　　src/components/ProjectManger/ScadaList.vue  
  　　src/components/Common/Modal.vue  
  　　src/components/Common/ConfigModal.vue   
  Vue-router  
  　　src/router/index.js   
  - UI:  
  ![](/assets/scada.png)  
  ![](/assets/scada_edit.png)  
  ![](/assets/scada_move.PNG)  
  ![](/assets/scada_delete.PNG)  
  ![](/assets/scada_create.PNG)      
  - 邏輯說明:  
        1. 在Device Management中編輯SCADA的頁面(Url: /Login/CloudManager/DeviceManagement/:projectId)。  
        　1.1 透過src/router/index.js呼叫src/components/ScadaList.vue進入可編輯SCADA的頁面，將$store.state.user.scope使用者的權限將變數存入scopeEdit。  
        2. 一進入即執行$checkLogin、getScadaList和getScadaStatus。  
        　2.1 執行$checkLogin判斷scopeEdit狀態。  
        　　2.1.1 若為true即可顯示5、6、7、9號按鈕。  
        　　2.1.2 若為false即不顯示5、6、7、9號按鈕。  
        　2.2 執行getScadaList列出所有SCADA相關資訊。  
        　　2.2.1 呼叫API: GET /Scadas/info/{projectId}。  
          　　2.2.1.1 若呼叫成功列出SCADA資料，並將要顯示資訊存到devices陣列中。  
          　　2.2.1.2 若呼叫失敗則判斷err.response.status === 404狀態。  
          　　　2.2.1.2.1 若為true，令devices = []。  
          　　　2.2.1.2.2 若為false，執行$checkToken。  
        3. 按下1號按鈕執行getScadaList並依使用者選擇或輸入做排序。  
        4. 按下2號按鈕執行handleDetailClick。  
        　4.1 呼叫API: /Scadas/info/{projectId}/{scadaId}。  
        　　4.1.1 若呼叫成功，令detailVisible = true，並將要顯示的資訊存到tempDevice物件中。  
        　　　4.1.1.1 呼叫src/components/Common/ConfigModal.vue，並傳入Config、typeEdit、obj、type等參數，分別為:Config="true" :typeEdit="scopeEdit" :obj="tempDevice" :type="'scada'"。  
        　　　　4.1.1.1.1 若呼叫成功則跳出detail視窗。  
        　　　　　4.1.1.1.1.1 若typeEdit=true即顯示9號按鈕，按下9號按鈕執行handleEditClick。  
        　　　　　　4.1.1.1.1.1.1 分別將顯示的參數存入showObj物件，編輯參數存入editObj物件，並判斷obj.alarm!==undefined。  
        　　　　　　　4.1.1.1.1.1.1.1 若為true將obj.alarm參數存入showAlarm和editAlarm。  
        　　　　　4.1.1.1.1.2 編輯完後按下12號按鈕執行handleSaveClick。  
        　　　　　　4.1.1.1.1.2.1 呼叫API: PUT /Scadas/{scadaId}。  
        　　　　　　　4.1.1.1.1.2.1.1 若呼叫成功即完成編輯，並回到一開始detail畫面。  
        　　　　　　　4.1.1.1.1.2.1.2 若呼叫失敗回傳errMsg。  
        　　　　　4.1.1.1.1.3 若不想儲存編輯按下11號按鈕，令editObj = {}並回到一開始detail畫面。  
        　　　　　4.1.1.1.1.4 按下10號按鈕，令tempProject = {}並令detailVisible = False關閉視窗。  
        　　　4.1.2 若呼叫失敗執行$checkToken。  
        5. 3號燈號會由執行getScadaStatus的結果決定，若是$store.state.fullscreenLoading不為false和scadaId不為null即呼叫API: POST /Statuses/scada。  
        　5.1 若呼叫成功3號則會亮綠燈並執行setTimer。  
        　　5.1.1 setTimer:每1000毫秒會執行一次getScadaStatus。  
        　5.2 若呼叫失敗3號則會不亮燈並執行$checkToken和setTimer。  
        　　5.2.1 setTimer:每1000毫秒會執行一次getScadaStatus。  
        6. 按下4號按鈕執行handleMoveClick。  
        　6.1 執行getProjectList並令moveVisiable = true，將選取的scadaId傳入tempScadaId字串。  
        　　6.1.1 呼叫src/components/Common/Modal.vue，顯示move to視窗。  
        　　　6.1.1.1 呼叫API: /GET/Projects。  
        　　　　6.1.1.1.1 若呼叫成功將回傳資料存入projects陣列中，列在下選單。  
        　　　　6.1.1.1.2 若呼叫失敗判斷err.response.status === 404狀態。  
        　　　　　6.1.1.1.2.1 若為true，令projects = []。  
        　　　　　6.1.1.1.2.2 若為false，執行$checkToken。  
        　　6.1.2 選完project Id後按下14號按鈕執行handleMoveSubmit。  
        　　　6.1.2.1 呼叫API: POST/Scadas。  
        　　　　6.1.2.1.1 若呼叫成功即完成編輯，並令moveVisiable = false關閉畫面。  
        　　　　6.1.2.1.2 若呼叫失敗，執行$checkToken。  
        　　6.1.3 若不想儲存按下13號按鈕執行handleCancelClick。  
        　　　6.1.3.1 令projectIdSelect = ""，並令moveVisiable = false關閉畫面。   
        7. 按下5號按鈕執行handleDeleteClick。  
        　7.1　將選取的scada資料存入tempDevice，並令deleteVisible = true，顯示delete視窗。  
        　　7.1.1 呼叫src/components/Common/Modal.vue，顯示delete視窗。  
        　　　7.1.1.1 若確定要刪除按下15號按鈕，執行deleteScada並呼叫API: DELETE /Projects/{projectId}。  
        　　　　7.1.1.1.1 若呼叫成功，將畫面上選取的SCADA資訊刪除，並令deleteVisiable = false關閉視窗。  
        　　　　7.1.1.1.2 若呼叫失敗，顯示errStr，並令deleteVisiable = false關閉視窗。  
        　　　7.1.1.2 若不想要刪除按下16號按鈕執行handleCancelClick。  
        　　　　7.1.1.2.1 令tempDevice = {}，並令deleteVisiable = false關閉視窗。  
        8. 按下6號按鈕執行handleAddClick。  
        　8.1 令addVisible = true和tempDevice = {}。  
        　　8.1.1 呼叫src/components/Common/Modal.vue，顯示New SCADA視窗。  
        　　　8.1.1.1 按下18號按鈕執行handleAddSubmitClick。  
        　　　　8.1.1.1.1 判斷devices.projectId狀態。  
        　　　　　8.1.1.1.1.1 若為false，顯示errMsg。  
        　　　　　8.1.1.1.1.2 若為true，執行createNewScada。  
        　　　　　　8.1.1.1.1.2.1 呼叫API: POST //Scadas。  
        　　　　　　　8.1.1.1.1.2.1.1 若呼叫成功執行getScadaList，並令addVisible = false關閉視窗。  
        　　　　　　　8.1.1.1.1.2.1.2 若呼叫失敗顯示errMsg。  
         　　　　8.1.1.2 按下17號按鈕，執行handleCancelClick。  
         　　　　　8.1.1.2.1 令tempDevice = {}並令addVisible = false關閉視窗。  
        9. 按下7號按鈕執行handleSyncClick。  
        　9.1 判斷isModified狀態。  
        　　9.1.1 若為true，將有更改過的scada存入arr陣列，呼叫 API: POST/Scadas/sync。  
        　　　9.1.1.1 若呼叫成功，做同步動作，若res.data[idx].isSuccess為false回覆errStr。  
        　　　9.1.1.2 若呼叫失敗，執行$checkToken並顯示errStr。  
        10. 按下8號按鈕執行handleDeviceClick。  
        　10.1 透過src/router/index.js呼叫src/components/DeviceList.vue進入可編輯Device的頁面。        
        
       
          
        





