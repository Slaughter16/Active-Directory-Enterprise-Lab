# Step 5: Quotas & File Screening (FSRM)

In this step, we will use **File Server Resource Manager (FSRM)** to enforce storage management policies on our file server. This includes:  

- **Quotas** → Limit the amount of storage space users or folders can consume.  
- **File Screening** → Restrict the types of files users can store (e.g., block media, executables).  
- **Notifications & Reports** → Configure alerts when thresholds are reached.  


---

## 1. Install File Server Resource Manager

1. Open **Server Manager** → **Manage** → **Add Roles and Features**.  
2. Leave **Installation Type** as *Role-based or feature-based installation* → click **Next**.  
3. Leave **Server Selection** default (local server) → click **Next**.  
4. Under **Server Roles**:  
   - Expand **File and iSCSI Services**.  
   - Check **File Server Resource Manager**.  
   - When prompted, click **Add Features**.  
   - Continue clicking **Next** until the confirmation screen.  
5. Click **Install**.  
6. Once installed, verify FSRM is available under:  
   - **Tools → File Server Resource Manager** in Server Manager.  

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
- Test the file screen by attempting to save a blocked file type (example: `.mp3`, `.exe`) → should be denied.  
- Check **notifications** or **event logs** if thresholds are configured.  

---

✅ At this point, **FSRM is successfully configured** to enforce storage limits and block unwanted files in shared folders.  
