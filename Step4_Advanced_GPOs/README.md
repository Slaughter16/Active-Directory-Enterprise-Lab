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
1️⃣ Create a folder for wallpapers 
On your Windows Server, open File Explorer.


Navigate to a drive you want to use (for example, C:\).


Right-click → New → Folder.


Name it something clear, e.g.:

 Wallpapers


Now you have a dedicated folder to store all GPO wallpapers.



Share the folder:

Right-click → Properties → Sharing → Advanced Sharing → Share this folder

2️⃣ Share the folder

Right-click D:\Wallpapers → Properties → Sharing → Advanced Sharing

Check Share this folder

Give Read permissions to the users/groups that need it (e.g., IT group)

Note the UNC path, e.g.:

\\DC01\Wallpapers\ITBackground.jpg3️⃣ Apply the wallpaper via GPO

Open Group Policy Management → Your IT OU GPO

Navigate to:

User Configuration → Policies → Administrative Templates → Desktop → Desktop → Desktop Wallpaper


Double-click → Enable

In Wallpaper Name, enter the UNC path from step 2:

\\DC01\Wallpapers\ITBackground.jpg


Set Wallpaper Style (Stretch, Fill, Center, etc.)

Click OK → Link GPO if not already linked

Test with one client first to make sure it resolves and users can see the image.

✅ Pros: Easy to manage centrally, updates automatically if you replace the image
❌ Cons: Users need network access to see it
   - Example: `\\Server01\Wallpapers\IT_Wallpaper.jpg`  
   - This ensures IT staff see a standardized background (branding, security reminder, etc.).




















2. **Disable Control Panel access**  
   - Path: `User Configuration → Administrative Templates → Control Panel → Prohibit access to Control Panel`  
   - Helps prevent IT staff from making unauthorized system changes.


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
