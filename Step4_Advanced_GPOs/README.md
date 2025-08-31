# 🖥️ Step 4 – Advanced Group Policy Objects (GPOs)

In this step, we will **create and link Group Policies** for different OUs (IT and HR).  
This simulates real-world administrative tasks where IT departments enforce policies for security, usability, and compliance.    

---


## 🛠️ Policies to Configure

### 🔹 IT Department (OU: LabUsers → IT)
**GPO Name:** `IT_User_Policy`

1. **Set a custom desktop background**  
   - Path: `User Configuration → Policies → Administrative Templates → Desktop → Desktop Wallpaper`
   - Use a Network share create a folder on your server : D:\Wallpapers
   - Copy your wallpaper image into that folder. Move or copy the image you downloaded from Firefox into that folder.
  
2. **Disable Control Panel access**  
   - Path: `User Configuration → Policies → Administrative Templates → Control Panel → Prohibit access to Control Panel`  
   - This prevents IT staff from making unauthorized changes.




## 1️⃣ Create a Wallpaper Folder

1. Open **File Explorer** → navigate to `C:\`  
2. Click **Home → New Folder** (or press **Ctrl + Shift + N**)  
3. Name the folder:  
- Wallpapers

![New_Folder_Wallpapers](images/1_New_Folder_Wallpapers.png)

Now you have a dedicated folder to store all GPO wallpapers.

4. Save the wallpaper image into this folder  
   - Example: `C:\Wallpapers\ITBackground.jpg`   
![Save_Image_Wallpapers](images/2_Save_Image_Wallpapers.png)

---

## 2️⃣ Share the Folder

Right-click → Properties → Sharing → Advanced Sharing → Share this folder

1. Right-click `C:\Wallpapers` → **Properties → Sharing → Advanced Sharing…**

![Sharing_Folder](images/3_Sharing_Folder.png)
![Advanced_Sharing](images/4_Advanced_Sharing.png)

   
3. Check **Share this folder**  
4. Click **Permissions → Add… → type IT_Staff → OK**  
5. Give **Read** permission → **Apply → OK**  
6. UNC path for GPO: 

\\WIN-Server\Wallpapers\ITBackground.jpg

![Object_Name_IT_Staff](images/5_Object_Name_IT_Staff.png)
![Share_Permissions_Wallpapers](images/6_Share_Permissions_Wallpapers.png)



3️⃣ Apply the wallpaper via GPO

Open Group Policy Management → Your IT OU GPO

![Edit_IT_User_Policy](images/7_Edit_IT_User_Policy.png) 

Navigate to:

User Configuration → Policies → Administrative Templates → Desktop → Desktop → Desktop Wallpaper

![Desktop_Wallpaper](images/8_Desktop_Wallpaper.png) 


- Double-click → Enable

- In Wallpaper Name, enter the UNC path from step 2:

- \\Win_Server\Wallpapers\ITBackground.jpg


- Set Wallpaper Style (Stretch, Fill, Center, etc.)

- Click OK → Link GPO if not already linked

![Apply_Wallpaper](images/9_Apply_Wallpaper.png) 


- Test with one client first to make sure it resolves and users can see the image.
- Log into WIN-11 client using Alice IT account and notice the background change 

![Login_Alice_WIN11](images/10_Login_Alice_WIN11.png) 

![Verify_AliceIT_Background](images/11_Verify_AliceIT_Background.png) 
 

✅ Pros: Easy to manage centrally, updates automatically if you replace the image
❌ Cons: Users need network access to see it
   - Example: `\\WIN-Server\Wallpapers\IT_Wallpaper.jpg`  
   - This ensures IT staff see a standardized background (branding, security reminder, etc.).


# 🖥️ IT GPO – Disable Control Panel Access

This section shows how to **disable Control Panel access** for IT users (e.g., AliceIT) using the existing IT_User_Policy GPO.

---

## 1️⃣ Open the GPO

1. Open **Group Policy Management**
![GPO_IT_Control_Panel](images/12_GPO_IT_Control_Panel.png) 

   
3. Navigate to:   LabUsers → IT OU → IT_User_Policy → Edit
![IT_Policy_Control_Panel](images/13_IT_Policy_Control_Panel.png) 


---

## 2️⃣ Locate the Setting

Navigate in the GPO Editor:   User Configuration → Policies → Administrative Templates → Control Panel → Prohibit access to Control Panel

![Prohibit_Access_Control_Panel](images/14_Prohibit_Access_Control_Panel.png) 

---

## 3️⃣ Configure the Policy

1. Double-click **Prohibit access to Control Panel**  
2. Select **Enabled**  
3. Click **Apply → OK**  

![Apply_Control_Panel_Policy](images/15_Apply_Control_Panel_Policy.png) 

![Verify_Control_Enabled](images/16_Verify_Control_Enabled.png) 

---

## 4️⃣ Test the Policy

1. Log in on a client machine as **AliceIT** (member of IT_Staff)  
2. Open Command Prompt → run:   gpupdate /force

![AliceIT_Update_Policy](images/17_AliceIT_Update_Policy.png) 



3. Try opening **Control Panel** → access should now be blocked  

![Open_Control_Panel](images/18_Open_Control_Panel.png) 
![Verify_ControlPanel_Prohibited](images/19_Verify_ControlPanel_Prohibited.png) 


---

## ✅ Notes

- This applies to all users in the **IT_Staff** group  
- Combined with the wallpaper policy, IT_User_Policy now enforces both branding and system restrictions




---

### 🔹 HR Department (OU: LabUsers → HR)
**GPO Name:** `HR_User_Policy`

We will configure:

1. **Redirect Documents folder** → central server share (`\\WIN-Server\HRDocs`)  
2. **Password-protected screensaver** → auto-lock after 5 minutes  
3. **Disable USB storage** → prevent data exfiltration
4. **Shared Folder** (\\WIN-SERVER\HRData$

---

 # 📂 Folder Redirection Setup (HR Department)

This guide explains how to configure Folder Redirection in Active Directory for the **HR department**.  
The configuration redirects each user’s **Documents** folder to a centralized file server share.

---

## 🖥️ 1. Create File Share on Server

1. Open **Server Manager** → **File and Storage Services** → **Shares**.  
2. Click **Tasks > New Share** → select:
   
![Tasks_NewShare](images/20_Tasks_NewShare.png) 


   - **SMB Share – Quick** (default), or  
   - **SMB Share – Advanced** if using File Server Resource Manager.


![SMB_Share_Quick](images/21_SMB_Share_Quick.png) 

4. Select **Server** = `WIN-Server`, **Volume** = `C:`.

![SMB_Share_Quick](images/22_Browse_Path.png) 
![Share_Browse](images/23_FOLDERREDIR_Select_Folder.png) 


6. Enter the following details:
   - **Local path**: `C:\FOLDERREDIR`
  
![FOLDERREDIR_Select_Folder](images/24_Next_Path.png) 

   - **Share name**: `FOLDERREDIR$`  
     - The `$` hides the share from casual browsing.
![Share_Name](images/25_Share_Name.png) 
   - **Protocol**: SMB  
   - Enable **Access-based enumeration** (recommended).  
   - Disable **Continuous Availability**.  
![Other_Settings](images/26_Other_Settings.png) 



2. Right-click → **Properties → Sharing → Advanced Sharing…**  
   - Share name: `HRDocs`  
   - Permissions: Add **HR_Staff group → Read/Write**





     
---

## 🔒 2. Configure NTFS Permissions

1. On the **Permissions** page, choose **Customize permissions**.  
2. Open **Advanced Security Settings** → **Disable inheritance** →  
   select **Convert inherited permissions into explicit permissions**.
   ![Disable_Inheritance](images/27_Disable_Inheritance.png) 
  
4. Configure the following permissions:

| Account / Group          | Permission                                                                 | Applies to                          |
|---------------------------|---------------------------------------------------------------------------|-------------------------------------|
| **SYSTEM**                | Full Control                                                             | This folder, subfolders, and files |
| **Administrators**        | Full Control                                                             | This folder, subfolders, and files |
| **Creator Owner**         | Full Control                                                             | Subfolders and files only           |
| **CORP\HR_Staff**         | List folder / Read data, Create folders / Append data, Read attributes, Read extended attributes, Read permissions | This folder only |

✅ Remove any other accounts not listed above.  

   ![HR_Staff_Permissions](images/28_HR_Staff_Permissions.png) 
      ![Permissions_Wizard](images/29_Permissions_Wizard.png) 

-Confirm Selections

   ![Confirm_Selections](images/30_Confirm_Selections.png) 
   ![Share_Success](images/31_Share_Success.png) 


-Confirm ShareFolder created
   ![Verify_ShareFolder](images/32_Verify_ShareFolder.png) 







---

## 📑 3. Configure Group Policy

1. In **Group Policy Management**, right-click the GPO for HR users (e.g. `HR_User_Policy`) → **Edit**.

   ![Group_PolicyMgmt](images/33_Group_PolicyMgmt.png) 
   ![Edit_HR_User_Policy.png](images/34_Edit_HR_User_Policy.png) 



2. Navigate to:  User Configuration > Policies > Windows Settings > Folder Redirection
   ![Folder_Redirection_Documents](images/35_Folder_Redirection_Documents.png)

3. Right-click **Documents** → **Properties**.
   ![Properties_Documents](images/36_Properties_Documents.png)



4. Configure as follows:
- **Setting**: `Basic - Redirect everyone’s folder to the same location`  
- **Target folder location**: `Create a folder for each user under the root path`  
- **Root path**: `\\WIN-Server\FOLDERREDIR$`  
- **Policy Removal**: `Redirect the folder back to the local userprofile location when the policy is removed` (recommended).  
5. Click **OK** → confirm the warning.  
   ![Target_Documents](images/37_Target_Documents.png)

---

---
 - Options:
     - ✅ Grant the user exclusive rights to Documents (optional, stricter security)
     - ❌ Or uncheck if admin visibility is required
### ⚠️ Exclusive Rights Setting
- In **Folder Redirection → Properties → Settings tab**, uncheck:  
**“Grant the user exclusive rights to Documents.”**

   ![Settings_Documents](images/38_Settings_Documents.png)

- **Why disable it?**  
- With it **enabled**, Windows tries to set the user as the folder **owner**, which can fail if the folder is owned by Administrators.  
- With it **disabled**, Windows just creates `\\WIN-SERVER\HRDocs\%USERNAME%` and redirects Documents without ownership conflicts.  


In this lab, “Grant the user exclusive rights to Documents” is unchecked to simplify folder creation and avoid ownership conflicts.
In a real-world environment, this setting would typically be enabled for user privacy, with proper NTFS/ownership configuration.
✅ **Best Practice:** Always disable this in lab/test environments unless you specifically need per-user ownership.

---

### Verification Steps
1.---

## ✅ 4. Test on Client

1. On an HR client (e.g. `Win10-HR`), log in as an HR user.
   ![Log_Into_EveHR](images/39_Log_Into_EveHR.png)




2. Run:  gpupdate /force
   ![Gpupdate_Force_FolderRedirect](images/40_Gpupdate_Force_FolderRedirect.png)

3. Open **Documents** – it should now have a green sync icon and be redirected to the server share.  and create Test folder in it

   ![Verify_DocumentsRedirection_Test](images/41_Verify_DocumentsRedirection_Test.png)

-verify the Location of Documents Redirection Folder in WIN 10 client 
      ![Verify_Location_Documents_EveHR](images/42_Verify_Location_Documents_EveHR.png)

5. On the server, verify that a subfolder with the user’s name was created in: C:\FOLDERREDIR

![WinServer_FOLDERREDIR](images/43_WinServer_FOLDERREDIR.png)
![Verify_EveHR_Folder](images/44_Verify_EveHR_Folder.png)
![Documents_EveHR](images/45_Documents_EveHR.png)
![Test_Folder_WinServer](images/46_Test_Folder_WinServer.png)



---

## 🔧 Troubleshooting

- **User cannot access redirected folder**  
→ Check NTFS permissions and ensure `CORP\HR_Staff` group is set correctly.  

- **No folder created on server**  
→ Run `gpresult /r` on the client to confirm the GPO applied.  

- **Offline files sync issues**  
→ Disable Offline Files in Control Panel if not required.  

- **Want to undo Folder Redirection?**  
→ Remove the GPO and select **Redirect the folder back to the local user profile location**.  

---

















## 2️⃣ Password-Protected Screensaver (HR_User_Policy)

---
## Configuration Steps

### 1. Open Group Policy Management
1. Launch **Server Manager → Tools → Group Policy Management**
![Tools_GroupPolicy_Password](images/58_Tools_GroupPolicy_Password.png)

2. Navigate to the HR OU and locate `HR_User_Policy`
![HR_User_Policy_Password.png](images/59_HR_User_Policy_Password.png)

### 2. Edit the GPO
1. Right-click the GPO → **Edit**
![Edit_HR_User_Policy_AdminTemp](images/47_Edit_HR_User_Policy_AdminTemp.png)

   
3. Navigate to:  User Configuration → Administrative Templates → Control Panel → Personalization
![ControlPanel_Personalization_](images/48_ControlPanel_Personalization.png)

### 3. Enable Password Protection
1. Double-click **Password protect the screen saver** → select **Enabled** → click **OK**
![Enable_PasswordProtect](images/49_Enable_PasswordProtect.png)
### 4. Set Screensaver Timeout
1. Double-click **Password protect the screen saver** → select **Enabled** → click **OK**
2. Double-click **Screen saver timeout** → enter inactivity period in seconds (e.g., `120` for 2 minutes) → click **OK**
![Enable_ScreenSaver_120sec](images/50_Enable_ScreenSaver_120sec.png)


### 6. Apply and Close
- Close the Group Policy Management Editor  
- The GPO is now configured and linked to the HR OU
![Apply&Close](images/51_Apply&Close.png)
---

## Verification on HR Client (EveHR – Windows 10)

1. Log in as **EveHR** on the HR client machine  
2. Update Group Policy and verify application:
```powershell
gpupdate /force
gpresult /r
```
![gpuupdate_HR_PasswordPolicy](images/52_gpuupdate_HR_PasswordPolicy.png)
![gpresult_HR_PasswordPolicy](images/53_gpresult_HR_PasswordPolicy.png)


3. Check Screensaver Settings: Go to: Control Panel → Personalization → Lock Screen → Screen Saver Settings
Verify:
-Screensaver is enabled
-Password protected is active
-Timeout matches the configured period

![Screensaver_Settings_EveHR](images/54_Screensaver_Settings_EveHR.png)

4. Stay on homepage inactive for 2mins for policy to apply 

![Homescreen_HR](images/55_Homescreen_HR.png)
![PasswordProtect_Applied](images/56_PasswordProtect_Applied.png)
![Back_To_LoginEveHR](images/57_Back_To_LoginEveHR.png)




3️⃣ Disable USB Storage for HR Users

## Objective
Prevent users in the HR department from using USB storage devices to help secure sensitive data.

---

## Steps to Configure USB Storage Restriction

### 1️⃣ Open Group Policy Management
1. Log in to the **Domain Controller**.
2. Open **Server Manager → Tools → Group Policy Management**.
![Tools_GroupPolicy_Password](images/58_Tools_GroupPolicy_Password.png)


---

### 2️⃣ Locate HR User Policy
1. Navigate to **Group Policy Objects**.
2. Find the **HR_User_Policy** linked to the `HR` OU.
3. Right-click → **Edit**.
![HR_User_Policy_Password](images/59_HR_User_Policy_Password.png)
---

### 3️⃣ Configure USB Storage Restriction
1. In the **Group Policy Management Editor**:
   - Navigate to:  
     `User Configuration → Policies → Administrative Templates → System → Removable Storage Access`
2. Enable the following policies:'
   - **Removable Disks: Deny write access** → Enabled
   - **Removable Disks: Deny read access** → Enabled


![Deny_Write_Access](images/60_Deny_Write_Access.png)
![Deny_Read_Access](images/61_Deny_Read_Access.png)
4. Verify the Deny rules are enabled.
![Verify_deny_enabled](images/62_verify_deny_enabled.png)

---

### 4️⃣ Apply and Update Policy
1. On the client machine (e.g., EveHR):
   - Open **Command Prompt** → Run `gpupdate /force`  
     ```
     gpupdate /force
     ```
     ![gpupdate_USB_restriction](images/63_gpupdate_USB_restrict.png)

2. Log off and log back on (if needed) for policy to take effect.

---

### 5️⃣ Verification
1. On the HR client:
   -Check with rsop.msc (Resultant Set of Policy)
On the client → press Win + R → type rsop.msc → Enter.
    ![rsop.msc_command](images/64_rsop.msc_command.png)

Navigate to:
User Configuration → Administrative Templates → System → Removable Storage Access
Verify Deny read / Deny write are showing as Enabled.
 ![Resultant_Set_Of_Policy](images/65_resultant_set_of_policy.png)

   - 
3. Run `gpresult /h report.html' then open 'report.html' in Command Prompt to confirm that **HR_User_Policy** is applied.Open the report.html file → look under User Configuration → Administrative Templates → System → Removable Storage Access.
It should show your configured settings as Enabled.
![gpresult/h_report.html](images/66_gpresulth.png)
 ![verify_report.html](images/67_report.html.png)




3.Registry Check (Optional but Screenshot-Friendly)

Folder Redirection & Removable Storage policies also write to the registry.
Open 'regedit' on the client and check:Check registry keys for confirmation:
HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\RemovableStorageDevices Look for entries showing Deny Read/Write policies.

![regedit_command](images/68_regedit_command.png)
![registry_editor](images/69_registry_edit.png)

---

## Notes
- Even if no physical USB is present, the policy can be verified via **Device Manager** or `gpresult` or 'rsop.msc'.
- This configuration is part of standard HR security measures to prevent data exfiltration.


---

# HR Shared Folder Setup

## Objective
Create a secure shared folder on the server for HR users, allowing only authorized HR staff to access and modify files. Verify proper permissions and client access.

---

---

## 1. Create the Shared Folder on the Server

1. Open Server Manager → File and Storage Services → Shares.  
2. Click Tasks → New Share.
   ![New_HRShared_Folder](images/70_New_HRShared_Folder.png)  
3. Select SMB Share – Quick (or Advanced if using File Server Resource Manager).
![Select_Profile_Shares](images/71_Select_Profile_Shares.png)
4. On Share Location, select the volume (e.g., C:).

5. On Share Name, enter  HRData$
   - The `$` makes the share **hidden** from casual browsing.
![Share_Name_HRData$](images/72_Share_Name_HRData$.png)
6. Other settings:  
- Enable **access-based enumeration**  
- Optionally enable **encryption**  
- Leave **continuous availability** unchecked

![Other_Settings_HRData](images/73_Other_Settings_HRData.png)


---

## 2. Configure NTFS Security Permissions

1. In **Advanced Security Settings** for `C:\Shares\HRData$`:  
- **Disable inheritance**  
- **Convert inherited permissions into explicit permissions**
  ![Permissions_HRData](images/74_Permissions_HRData.png)
  ![Disable_Inheritance_HRData](images/75_Disable_Inheritance_HRData.png)
2. Set permissions:

| Account / Group       | Permissions                                   | Applies To |
|----------------------|-----------------------------------------------|------------|
| `SYSTEM`             | Full Control                                  | This folder, subfolders, and files |
| `BUILTIN\Administrators` | Full Control                             | This folder only |
| `CREATOR OWNER`       | Full Control                                  | Subfolders and files only |
| `CORP\HR_Staff`       | List folder / read data, create folders / append data, read attributes, read extended attributes, read permissions | This folder only |
| Other accounts        | None (remove any not listed above)           | -          |

![Add_HR_Staff_HRData](images/76_Add_HR_Staff_HRData.png)
![Edit_Permissions_HRData](images/77_Edit_Permissions_HRData.png)
![Verify_Permissions_HRData](images/78_Verify_Permissions_HRData.png)
![Next_Permissions_HRData](images/79_Next_Permissions_HRData.png)
> Only HR staff can create/modify files; other users are denied access.

## 3. Click Create Folder & Verify Successfully created
![Create_HRData$](images/80_Create_HRData$.png)
![Success_Created_HRData](images/81_Success_Created_HRData.png)

---

## 3. Verify Share Permissions

#### On Windows Client (EveHR)

1. Open File Explorer.

2. Enter the UNC path in the address bar: \\WIN-SERVER\HRData$
3. HR users should see the shared folder.
![WIN_SERVER_HRData$](images/82_WIN_SERVER_HRData$.png)
![HRData$_Folder](images/83_HRData$_Folder.png)
5.  Create a test file/folder (e.g., `Test1`) on the client inside `HRData$`.
![Test1_File](images/84_Test1_File.png)
HR user should be able to create/edit files.

Non-HR users should not have access.

### Optional: Manually Map as Network Drive or via GPO

---

#### Manual Map

1. Right-click This PC → Map Network Drive.

2. Select a drive letter (Z:) and enter: \\WIN-SERVER\HRData$

3. Check Reconnect at sign-in.

4. Verify access for HR users.
![Z_Drive_Letter](images/85_Z_Drive_Letter.png)
![Verify_Test1_WIN_10](images/86_Verify_Test1_WIN_SERVER.png)

---

#### Map via GPO

1. Open Group Policy Management on the server.Edit the HR_User_Policy (linked to HR OU / LabUsers HR).

2.Navigate to:
User Configuration → Preferences → Windows Settings → Drive Maps
![Drivemaps_before_config](images/90_drivemaps_before_config.png)

3.Right-click → New → Mapped Drive.
![New_mapped_drive](images/91_new_mapped_drive.png)

4.Configure:
- Location: \\WIN-SERVER\HRData$
- Drive Letter: Z:
- Action: Create (or Replace if you want to overwrite old mapping)
- Optional: Check Reconnect at logon.
- Apply and OK.
![New drive properties](images/92_New_drive_properties.png)

5. Verify mapped creation
![Drive maps Z:](images/93_Drive_maps_Z:.png)
#### WIN-SERVER verification

- Verify WIN-SERVER can view the test file created in the shared folder from the client- ![Verify_Test1_WIN_SERVER](images/87_Verify_Test1_WIN_SERVER.png)
---



  















## 🎯 Real-World Notes

- These examples are **realistic** policies seen in corporate environments.  
- IT policies often **restrict customization** to maintain security and branding.  
- HR policies usually focus on **data protection** and compliance (folder redirection + screensavers).  
- In a real job, you’d expand this with policies for software deployment, security hardening, and compliance requirements.

---
✅ At the end of Step 4, your lab will have working **department-specific policies** applied to users.

---
