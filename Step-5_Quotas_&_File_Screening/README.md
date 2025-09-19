# 🗂️ Step 5: Quotas & File Screening (FSRM)

## 📑 Table of Contents

1. 🎯 [Objectives](#objectives)
2. 🛠️ [Install File Server Resource Manager (FSRM)](#1-install-file-server-resource-manager)
3. 🗄️ [Configure Quotas](#2-configure-quotas)
   - [Create Quota](#create-quota)
   - [Define Custom Properties](#define-custom-properties)
   - [Configure Notification Thresholds](#configure-notification-thresholds)
   - [Save Quota Template](#save-quota-template)
4. 🚫 [Configure File Screening](#3-configure-file-screening)
   - [Create File Screen](#create-file-screen)
   - [Define Custom File Screen Properties](#define-custom-file-screen-properties)
   - [Active Screening & Blocked File Types](#active-screening--blocked-file-types)
   - [Save File Screen Template](#save-file-screen-template)
5. ✅ [Verification](#4-verification)
   - [Test Quota](#test-the-100-mb-quota)
   - [Test File Screening](#test-the-file-screen)
   - [Verify Quota Alerts in Event Viewer](#verify-quota-alerts-in-event-viewer)

--- 
<a id="objectives"></a>
## 🎯 Objectives
In this step, we will use **File Server Resource Manager (FSRM)** to enforce storage management policies on our file server. This includes:  

- **Quotas** → Limit the amount of storage space users or folders can consume.  
- **File Screening** → Restrict the types of files users can store (e.g., block media, executables).  
- **Notifications & Reports** → Configure alerts when thresholds are reached.  


---
<a id="install-file-server-resource-manager"></a>
## 🛠️ 1. Install File Server Resource Manager

1. Open **Server Manager** → **Manage** → **Add Roles and Features**.

![Add_Roles_Features](images/1_Add_Roles_Features.png)
2. Leave **Installation Type** as *Role-based or feature-based installation* → click **Next**.  
![Installation_Type](images/2_Installation_Type.png)


3. Leave **Server Selection** default (local server) → click **Next**.

![Server_Selection](images/3_Server_Selection.png)
4. Under **Server Roles**:  
   - Expand **File and iSCSI Services**.  
   - Check **File Server Resource Manager**.  
   - When prompted, click **Add Features**.  
   - Continue clicking **Next** until the confirmation screen.

     ![Server_Roles](images/4_Server_Roles.png)
     ![Check_FSRM](images/5_Check_FSRM.png)
     ![Add_Features_FSRM](images/6_Add_Features_FSRM.png)
     ![Features_Next](images/7_Features_Next.png)
5. Click **Install** and Verify Installation.

![Confirmation_Install](images/8_Confirmation_Install.png)
![Result_FSRM](images/9_Result_FSRM.png)
 

---
<a id="configure-quotas"></a>
## 🗄️ 2. Configure Quotas

Quotas allow administrators to restrict the amount of data stored in a folder.  
<a id="create-quota"></a>
1. Open **File Server Resource Manager** → expand **Quota Management**.

![Open_FSRM](images/10_Open_FSRM.png)
![FSRM_Dashboard](images/11_FSRM_Dashboard.png)
2. Right-click **Quotas** → select **Create Quota**.  

![Create_Quota](images/12_Create_Quota.png)
<a id="define-custom-properties"></a>
3. In the **Quota Path**, select the target shared folder (example: `D:\Shares\HRData$`). 
![Quota_Path](images/13_Quota_Path.png)
4. Choose **Define custom quota properties**.  
![Custom_Properties](images/14_Custom_Properties.png)
5. Configure settings:  
   - **Hard quota** (recommended): Prevents users from exceeding the limit.  
   - Set a space limit (example: `500 MB` or `1 GB`).  
   - Add a description for documentation.

![Hard_Quota](images/15_Hard_Quota.png)
6. (Optional) Configure **Notification Thresholds**:  
   - Click **Add** → set threshold (example: `80%`).  
   - Choose actions:  
     - Send email to administrators  
     - Log an event  
     - Run a command  
     - Generate a report  
![Add_Threshold](images/16_Add_Threshold.png)
![Click_Yes_NoSMTP](images/17_Click_Yes_NoSMTP.png)
![Quota_Properties_Check](images/18_Quota_Properties_Check.png)

7. Create Quota, Save Template as (HRData_Quota), Verify Quota created)


![Create_HRData_Quota](images/19_Create_HRData_Quota.png)
![Save_Template_Name](images/20_Save_Template_Name.png)
![Verify_Quota_Created](images/21_Verify_Quota_Created.png)

---

## 3. Configure File Screening

File Screening allows administrators to block specific file types in shared folders.  

1. In **FSRM**, expand **File Screening Management**.
![File_Screening_Management](images/22_File_Screening_Management.png)

2. Right-click **File Screens** → select **Create File Screen**.
![Create_File_Screen](images/23_Create_File_Screen.png)
 
3. Select the target folder path (example: `D:\Shares\HRData$`).  
4. Choose configuration method:  
   - **Derive from a template** (preconfigured rules), or  
   - **Define custom file screen properties** (granular control).
     ![Define_Custom_File_Screen_Prop](images/24_Define_Custom_File_Screen_Prop.png)
5. For this lab, define a **custom file screen**:  
   - Select **Active Screening** (prevents saving disallowed files).  
   - Block categories such as:  
     - Audio and Video Files  
     - Compressed Files  
     - Executable Files  
     - Image Files  
     - Web Page Files  
   - This ensures only text/document file types are allowed in the HR share.
   ![Active_Screening_HRData](images/25_Active_Screening_HRData.png)
   ![File_Groups_Block](images/26_File_Groups_Block.png)
6. (Optional) Save this configuration as a **custom template** for reuse.

![Create_HRData_FileScreen](images/27_Create_HRData_FileScreen.png)
![Template_FileScreen](images/28_Template_FileScreen.png)
7. Click **OK** → verify the file screen is created.  
![Verify_FileScreen_Created](images/29_Verify_FileScreen_Created.png)

---

## 4. Verification

### Test the 100 MB Quota

On the **EveHR** (Win10 client):
Test the 100 MB Quota

On EveHR (Win10 client):

1. Open **Command Prompt**.
2. Create a 50 MB file in the mapped HR share (replace `Z:` with your mapped drive letter):

   ```bash
   fsutil file createnew Z:\test1.dat 52428800
   create another 60 MB file
   fsutil file createnew Z:\test2.dat 62914560
   ```

When you try to create the second file, it should fail because you’ve exceeded the 100 MB quota.

You should get an error like “Not enough disk space”.

![Exceed_100MB](images/30_Exceed_100MB.png)

- Open FSRM on the server and Verify the quota usage percentage for C:\Shares\HRData$
 ![Quota_Usage_Percentage](images/35_Quota_Usage_Percentage.png)


### Test the File Screen

The file screen is configured to block certain file types (e.g., `.exe`, `.mp3`).

#### Method 1: Create a Dummy EXE

1. Open **Notepad**.  
   Type `Test.exe`.  
![Notepad_TestExe](images/31_Notepad_TestExe.png)
2. Go to **File → Save As**.  
   - Change **Save as type** to **All Files**.  
   - Enter the name `test.exe`.
3. Save the file to the mapped HR share (`Z:\`).

![Save_Exe_File](images/32_Save_Exe_File.png)
![Save_File_To_HRData_Z:](images/33_Save_File_To_HRData_Z.png)

4. Attempting to save should be **blocked** with an "Access Denied" error.
![Exe_File_Denied](images/34_Exe_File_Denied.png)

#### Verify Quota Alerts in Event Viewer

- After testing the quota by filling the HRData share, you can confirm that the quota threshold events were logged.

1. Open Event Viewer
![Open_Event_Viewer](images/35_Open_Event_Viewer.png)

2. In the left panel, expand:
   - Custom Views → Administrative Events
![Custom_Views](images/36_Custom_Views.png)

3. Look for Warning Events related to File Server Resource Manager and Click the Warning to view detailed information `(User CORP\EveHR has exceeded the 80% quota threshold 
for the quota on C:\Shares\HRData$ on server WIN-SERVER)`
![Warning_Threshold](images/37_Warning_Threshold.png)

---

✅ At this point, **FSRM is successfully configured** to enforce storage limits and block unwanted files in shared folders.  
