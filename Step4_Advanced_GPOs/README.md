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

---

## 1️⃣ Folder Redirection – Documents → \\WIN-Server\HRDocs

### Server Setup

1. On your server, create a folder: C:\HRDocs

2. Right-click → **Properties → Sharing → Advanced Sharing…**  
   - Share name: `HRDocs`  
   - Permissions: Add **HR_Staff group → Read/Write**  
3. Set **NTFS Permissions** (Security tab):  
   - Add **HR_Staff → Full Control**  
   - Remove **Everyone** if present  
4. Note UNC path:  \\WIN-SERVER\HRDocs


``` ### GPO Configuration 1. Open **Group Policy Management → LabUsers → HR OU → HR_User_Policy → Edit** 
2. Navigate: ``` User Configuration → Policies → Windows Settings → Folder Redirection → Documents ```
3. Right-click **Documents → Properties** → Basic – Redirect everyone’s folder to the same location
4. Target folder location → **Redirect to the following location**
5. Set path: ``` \\WIN-Server\HRDocs ```
6. Apply → OK *(Optional screenshot placeholder)* ![HR_FolderRedirection](./images/hr-folderredirection.png) ---


## verify:

1️. On the HR client machine (e.g., EveHR)

Log in as the HR user (member of HR_Staff)

Open File Explorer

Navigate to Documents (or This PC → Documents)

✅ If the policy applied correctly, the path at the top should show the redirected location:

\\WIN-Server\HRDocs\EveHR


Tip: Windows automatically creates a subfolder for each user (%USERNAME%) if “Redirect to the following location” with user-specific subfolders is selected.

2️. Command-Line Verification

Open Command Prompt

Run:

gpresult /r


Look under User Settings → Folder Redirection

You should see that the Documents folder is redirected and the HR_User_Policy GPO is applied

3️. Manual Test

Try saving a file in Documents

Check the server folder (\\WIN-Server\HRDocs\EveHR)

The file should appear there, confirming redirection works

✅ Notes

If redirection didn’t apply:

Run gpupdate /force and log off/log back in

Make sure HR user is member of HR_Staff

Ensure the HRDocs folder has proper NTFS/share permissions










## 2️⃣ Password-Protected Screensaver
1. In `HR_User_Policy`, navigate: ``` User Configuration → Policies → Administrative Templates → Control Panel → Personalization ```
2. Enable these settings: - **Password protect the screen saver** → Enabled - **Screen saver timeout** → Enabled → 300 seconds (5 min) - **Force specific screen saver** → optional (`scrnsave.scr`)
3. Apply → OK *(Optional screenshot placeholder)* ![HR_Screensaver](./images/hr-screensaver.png) --- ##




3️⃣ Disable USB Storage
1. In `HR_User_Policy`, navigate: ``` Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access ```
2. Enable: - **All Removable Storage classes: Deny all access** → Enabled 3. Apply → OK *(Optional screenshot placeholder)* ![HR_USBBlock](./images/hr-usbblock.png) --- ##


4️⃣ Test HR Policy
1. Log in as an HR user (e.g., **EveHR**)
2. Run in Command Prompt: ``` gpupdate /force ```
3. Log off/log back in
4. Confirm: - Documents redirect to `\\WIN-Server\HRDocs` - Screensaver locks after 5 minutes -
USB storage is blocked *(Optional screenshot placeholder)*
![HR_Test](./images/hr-test.png) --- ##







✅ Notes - Policies apply to all members of the **HR_Staff** group - Folder redirection ensures HR documents are **centralized and backed up** - USB block prevents sensitive data exfiltration - Screensaver enforces workstation security compliance --- ## 🔹 Real-World IT Tasks Related to HR Policies - Resetting forgotten passwords - Unlocking accounts after lockout - Adjusting group memberships (HR_Staff, IT_Staff, etc.) - Fixing folder access issues - Deploying printers or drive mappings for HR users - Monitoring login activity and auditing These reflect **daily IT admin responsibilities** in a corporate environment. ``` --- This README covers all **three HR policies** plus **testing and real-world notes**, formatted for GitHub. If you want, I can **also make a matching IT + HR combined Step 4 README** that includes **both OUs in one document** for your repo.








1. **Redirect Documents folder to a shared folder**  
   - Path: `User Configuration → Windows Settings → Folder Redirection → Documents`  
   - Target: `\\Server01\HRDocs`  
   - Ensures HR data is stored centrally for backup/security.

2. **Set password-protected screensaver**  
   - Path: `User Configuration → Administrative Templates → Control Panel → Personalization → Password protect the screen saver`  
   - Timeout: `10 minutes`  
   - Prevents unauthorized access when HR staff leave their workstations.
  















## 🎯 Real-World Notes

- These examples are **realistic** policies seen in corporate environments.  
- IT policies often **restrict customization** to maintain security and branding.  
- HR policies usually focus on **data protection** and compliance (folder redirection + screensavers).  
- In a real job, you’d expand this with policies for software deployment, security hardening, and compliance requirements.

---
✅ At the end of Step 4, your lab will have working **department-specific policies** applied to users.

---
