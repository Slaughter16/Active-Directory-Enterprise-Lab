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

![New_Folder_Wallpapers](image/1_New_Folder_Wallpapers)

Now you have a dedicated folder to store all GPO wallpapers.

4. Save the wallpaper image into this folder  
   - Example: `C:\Wallpapers\ITBackground.jpg`   
![Save_Image_Wallpapers](image/2_Save_Image_Wallpapers)

---

## 2️⃣ Share the Folder

Right-click → Properties → Sharing → Advanced Sharing → Share this folder

1. Right-click `C:\Wallpapers` → **Properties → Sharing → Advanced Sharing…**

![Sharing_Folder](image/3_Sharing_Folder)
![Advanced_Sharing](image/4_Advanced_Sharing)

   
3. Check **Share this folder**  
4. Click **Permissions → Add… → type IT_Staff → OK**  
5. Give **Read** permission → **Apply → OK**  
6. UNC path for GPO: 

\\WIN-Server\Wallpapers\ITBackground.jpg

![Object_Name_IT_Staff](image/5_Object_Name_IT_Staff)
![Share_Permissions_Wallpapers](image/6_Share_Permissions_Wallpapers)



3️⃣ Apply the wallpaper via GPO

Open Group Policy Management → Your IT OU GPO

![Edit_IT_User_Policy](image/7_Edit_IT_User_Policy) 

Navigate to:

User Configuration → Policies → Administrative Templates → Desktop → Desktop → Desktop Wallpaper

![Desktop_Wallpaper](image/8_Desktop_Wallpaper) 


Double-click → Enable

In Wallpaper Name, enter the UNC path from step 2:

\\Win_Server\Wallpapers\ITBackground.jpg


Set Wallpaper Style (Stretch, Fill, Center, etc.)

Click OK → Link GPO if not already linked

![Apply_Wallpaper](image/9_Apply_Wallpaper) 


Test with one client first to make sure it resolves and users can see the image.
log into WIN-11 client using Alice IT account and notice the background change 

![Login_Alice_WIN11](image/10_Login_Alice_WIN11) 

![Verify_AliceIT_Background](image/11_Verify_AliceIT_Background) 
 

✅ Pros: Easy to manage centrally, updates automatically if you replace the image
❌ Cons: Users need network access to see it
   - Example: `\\WIN-Server\Wallpapers\IT_Wallpaper.jpg`  
   - This ensures IT staff see a standardized background (branding, security reminder, etc.).


# 🖥️ IT GPO – Disable Control Panel Access

This section shows how to **disable Control Panel access** for IT users (e.g., AliceIT) using the existing IT_User_Policy GPO.

---

## 1️⃣ Open the GPO

1. Open **Group Policy Management**  
2. Navigate to:   LabUsers → IT OU → IT_User_Policy → Edit

---

## 2️⃣ Locate the Setting

Navigate in the GPO Editor:   User Configuration → Policies → Administrative Templates → Control Panel → Prohibit access to Control Panel

---

## 3️⃣ Configure the Policy

1. Double-click **Prohibit access to Control Panel**  
2. Select **Enabled**  
3. Click **Apply → OK**  

*(Optional screenshot placeholder)*  
![DisableControlPanel](./images/disable-controlpanel.png)

---

## 4️⃣ Test the Policy

1. Log in on a client machine as **AliceIT** (member of IT_Staff)  
2. Open Command Prompt → run:   gpupdate /force

3. Try opening **Control Panel** → access should now be blocked  

*(Optional screenshot placeholder)*  
![TestControlPanel](./images/test-controlpanel.png)

---

## ✅ Notes

- This applies to all users in the **IT_Staff** group  
- Combined with the wallpaper policy, IT_User_Policy now enforces both branding and system restrictions









---

### 🔹 HR Department (OU: LabUsers → HR)
**GPO Name:** `HR_User_Policy`

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
