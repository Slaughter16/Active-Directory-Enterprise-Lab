# Step 5: Quotas & File Screening (FSRM)

In this step, we will use **File Server Resource Manager (FSRM)** to enforce storage management policies on our file server. This includes:  

- **Quotas** → Limit the amount of storage space users or folders can consume.  
- **File Screening** → Restrict the types of files users can store (e.g., block media, executables).  
- **Notifications & Reports** → Configure alerts when thresholds are reached.  


---

## 1. Install File Server Resource Manager

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

## 2. Configure Quotas

Quotas allow administrators to restrict the amount of data stored in a folder.  

1. Open **File Server Resource Manager** → expand **Quota Management**.  
2. Right-click **Quotas** → select **Create Quota**.  
3. In the **Quota Path**, select the target shared folder (example: `D:\Shares\HRData$`).  
4. Choose **Define custom quota properties**.  
5. Configure settings:  
   - **Hard quota** (recommended): Prevents users from exceeding the limit.  
   - Set a space limit (example: `500 MB` or `1 GB`).  
   - Add a description for documentation.  
6. (Optional) Configure **Notification Thresholds**:  
   - Click **Add** → set threshold (example: `80%`).  
   - Choose actions:  
     - Send email to administrators  
     - Log an event  
     - Run a command  
     - Generate a report  

---

## 3. Configure File Screening

File Screening allows administrators to block specific file types in shared folders.  

1. In **FSRM**, expand **File Screening Management**.  
2. Right-click **File Screens** → select **Create File Screen**.  
3. Select the target folder path (example: `D:\Shares\HRData$`).  
4. Choose configuration method:  
   - **Derive from a template** (preconfigured rules), or  
   - **Define custom file screen properties** (granular control).  
5. For this lab, define a **custom file screen**:  
   - Select **Active Screening** (prevents saving disallowed files).  
   - Block categories such as:  
     - Audio and Video Files  
     - Compressed Files  
     - Executable Files  
     - Image Files  
     - Web Page Files  
   - This ensures only text/document file types are allowed in the HR share.  
6. (Optional) Save this configuration as a **custom template** for reuse.  
7. Click **OK** → verify the file screen is created.  

---

## 4. Verification

- Test the quota by copying files into the shared folder until the limit is reached.

Test the 100 MB Quota

On EveHR (Win10 client):

Open Command Prompt.

Create a 50 MB file in the mapped HR share (replace Z: with your mapped drive letter): fsutil file createnew Z:\test1.dat 52428800 
(that’s 50 MB).

Create another 60 MB file:

fsutil file createnew Z:\test2.dat 62914560

When you try to create the second file, it should fail because you’ve exceeded the 100 MB quota.

You should get an error like “Not enough disk space”.






- Test the file screen by attempting to save a blocked file type (example: `.mp3`, `.exe`) → should be denied.



Test File Screening

Your screen blocks EXEs, MP3s, etc.

Method 1: Create a dummy EXE

Open Notepad → type random text.

Save As → change to All Files → name it test.exe.

Try copying it to Z:\ → should get Access Denied.


- Check **notifications** or **event logs** if thresholds are configured.  

---

✅ At this point, **FSRM is successfully configured** to enforce storage limits and block unwanted files in shared folders.  
