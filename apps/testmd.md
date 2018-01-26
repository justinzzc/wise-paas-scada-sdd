SCADA Portal User Guide      
=============

What is SCADA?      
------------

SCADA (Supervisory Control and Data Acquisition) is an automation management system of software and hardware elements for providing data acquisition, integration, analytics, and visualization technology in IoT.

The functionality of SCADA      
---------------

SCADA consists of three parts: cloud manager, account management.      
 

SCADA Cloud Manager      
=============

There are four layers in SCADA Cloud Manager: the project layer, the SCADA node layer, the device layer, and the tag layer.      
           
How to use the SCADA portal?      
--------------
**Getting Started**   
The procedure of WISE-PaaS connection setting.   
In the beginning, users have to configure the project and the SCADA node on the WISE-PaaS/SCADA portal.   
The user can use the SCADA Id to configure the connection setting on the WebAccess/SCADA.   
Then the user can synchronize the tag to the WISE-PaaS/SCADA though communicating the specified MQTT message format.

- Step1: Configure the project  on the  WISE-PaaS/SCADA portal
- Step2: Configure the SCADA node on the  WISE-PaaS/SCADA portal
- Step3: Configure the WISE-PaaS connection setting on the WebAccess/SCADA 
- Step4: Whitelist setting
- Step5: Upload the tag configuration and data with the specified MQTT message format
  

<br/>
<img src="../uploads/images/SCADA/35.jpg"  width="600"><br/>   



**General Usage-Flow**

- Sign in for SCADA portal      
- SCADA device management      
- SCADA account management   


**SCADA Menu Page**
<br/><img src="../uploads/images/SCADA/mainPg.jpg" width="70%"><br/>  

Sign in  the portal by clicking "Sign in" button    

The links of Cloud Manager or Dashboard are included in the menu page      

<br/><img src="../uploads/images/SCADA/devPg.jpg" width="70%"><br/>               

    

SCADA Device Management      
----------------
The default page is the "Device Management" page, or choose the "Device Management" item from function bar.    
Direct to the Device Management page by clicking "Device Management"   

<br/><img src="../uploads/images/SCADA/devPg2.jpg" width="70%"><br/>               
        

SCADA Device Management-Project      
--------------------
The Project list display on the cloud manager page      

<br/><img src="../uploads/images/SCADA/19.jpg"  width="70%"><br/>               

<br/>Create Project: Click the "New Project" button       

<br/><img src="../uploads/images/SCADA/20.jpg"  width="70%"><br/>              

Edit Project
The user can modify the project configuration by clicking "Edit" button.         
- Click the "..." button      
- Click the "Edit" button      
- Save the modified configuration <br/>         

<br/><img src="../uploads/images/SCADA/28.jpg">          


Delete Project 
When the project has been deleted. The SCADA nodes under the project would remove synchronically on the WebAccess/SCADA by clicking "Delete" button.    
- Click the "Delete" button 
            

Create SCADA: Click the "New SCADA" button       

<br/><img src="../uploads/images/SCADA/27.jpg"  width="70%"><br/>               

View SCADA: Click the ">" button          

<br/><img src="../uploads/images/SCADA/21.jpg"  width="70%"><br/>              

The SCADA list display on the cloud manager page      

<br/><img src="../uploads/images/SCADA/10.jpg"  width="70%"><br/>               

SCADA Device Management-SCADA      
--------------------

- The status of SCADA: "●" (Green is online, Gray is off-line)         
- Direct to the device list page: Click the ">" button         
- View SCADA information: Click the "..." button     

<br/><img src="../uploads/images/SCADA/12.jpg" width="600"><br/>               

Edit SCADA   
<br/> 
When the specific properties of the SCADA node have been modified, the SCADA node would update synchronically on the WebAccess/SCADA by clicking "Synchronize" button.      

- Click the "..." button      
- Click the "Edit" button      
- Save the modified configuration      


<br/> <img src="../uploads/images/SCADA/22.jpg"><br/>               

Delete SCADA
When the SCADA node has been deleted. The SCADA node would remove synchronically on the WebAccess/SCADA by clicking "Delete" button.       
- Click the "Delete" button

SCADA Device Management-Device      
-------------------

<img src="../uploads/images/SCADA/13.jpg" width="70%">         

- Model: device type (ex :WebAccess SCADA Device)      
- Direct to the tag list page: Click the ">" button      
- View device information: Click the "..." button      

<br/><img src="../uploads/images/SCADA/14.jpg" width="600"><br/>               

Edit Device
When the specific properties of devices have been modified, the device can be updated synchronically on the WebAccess/SCADA by clicking "Synchronize" button.       
- Click the "..." button
- Click the "Edit" button      
- Save the modified configuration      

<br/><img src="../uploads/images/SCADA/23.jpg"><br/>                     

Delete Device 
When the device has been deleted. The device would remove synchronically from on the WebAccess/SCADA by clicking "Synchronize" button.                    
- Click the "Delete" button

SCADA Device Management-Tag            
--------------

<br/><img src="../uploads/images/SCADA/15.jpg" width="70%"><br/>                  

- Type: The type of tag (Analog/Discrete/Text)            
- Unit: The engineering unit of Tag (ex: %)            
- View tag information: Click the "..." button            

Edit Tag  
When the specific properties of tags have been modified, the WISE-PaaS whitelist tag can be updated synchronically on the WebAccess/SCADA by clicking "Synchronize" button.            
- Click the "..." button            
- Click the "Edit" button            
- Save the modified configuration    
        
<br/><img src="../uploads/images/SCADA/24.jpg"> <br/>         

Edit Tag Alarm Properties    
When the specific alarm properties of tags have been modified, the WISE-PaaS whitelist tag can be updated synchronically on the WebAccess/SCADA by clicking "Synchronize" button.            
- Click the "..." button            
- Click the "Edit" button            
- Save the modified alarm configuration    
        
<br/><img src="../uploads/images/SCADA/alarm.jpg"> <br/>   

Delete Tag
When the tags have been deleted. The  WISE-PaaS whitelist tags would remove synchronically from on the WebAccess/SCADA by clicking "Synchronize" button.     
- Click the "Delete" button

SCADA Device Management-Synchronize            
--------------
Synchronize the modified configuration to the WebAccess/SCADA whitelist tags            
- Direct to the SCADA page            
- Click the "Synchronize" button <br/>  
          
<br/><img src="../uploads/images/SCADA/25.jpg" width="70%"><br/>          

SCADA Device Management-Filter           
--------------
Click the "Filter" button        

<br/><img src="../uploads/images/SCADA/29.jpg" width="60%">    


SCADA Account Management      
=============

Account Management     <br/>
----------------
Account setting and role setting are included in the account management    

<br/> <img src="../uploads/images/SCADA/34.jpg"  width="40%"><br/>     

Account Management-Account     
----------------


Create Account:    <br/>
Click the "Add New Account" button  
User can configure the  email address, role type, privilege for the account 

<br/> <img src="../uploads/images/SCADA/30.jpg"  width="70%"><br/>     

Edit Account:     <br/> 
Click the "Setting" button          

<br/> <img src="../uploads/images/SCADA/31.jpg"  width="70%"><br/>   

Delete Account   
- Click the "Delete" button

  
Account Management-Role    
----------------
<br/> <img src="../uploads/images/SCADA/36.jpg" >    <br/>    
 
Create Role: Click the "Add New Role" button   
There are four scopes of role setting:
- "edit_config": User has the authority to edit the configuration of the whole project
- "edit_value": User has the authority to edit the value of the tag
- "get_value": User has the authority to get the value of the tag
- "manage_account": User has the authority to manage the account on the WISE-PaaS/SCADA portal
      

<br/> <img src="../uploads/images/SCADA/32.jpg"  width="70%"><br/>     

Edit Role: Click the "Setting" button      


<br/> <img src="../uploads/images/SCADA/33.jpg"  width="70%"><br/>    


Delete Role   
- Click the "Delete" button
 
 
WebAccess Dashboard   
---------------- 
WISE-PaaS/Dashboard is based on the framework of open source Grafana.  
- Choose the "WebAccess Dashboard" item from function bar

<br/> <img src="../uploads/images/SCADA/37.jpg"  width="600">   <br/>    

User Guide 
---------------- 
- Choose the "User Guide" item from function bar
- Direct to the technical portal

<br/> <img src="../uploads/images/SCADA/38.jpg"  width="600">   <br/>  

API Document  
---------------- 
- Choose the "API Document" item from function bar
- Direct to the WISE-PaaS/SCADA API document   

<br/> <img src="../uploads/images/SCADA/39.jpg"  width="600">   <br/> 