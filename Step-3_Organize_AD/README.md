# Step 3 – Organize Active Directory, Create Users & Groups

## 📌 Objective
Organize Active Directory by creating Organizational Units (OUs), user accounts, and security groups.  
This enables structured administration, targeted Group Policy application, and role-based access management in your lab environment.

---

## 🔹 Background
- **Organizational Units (OUs):** Logical containers in AD to group users, computers, and groups.  
- **User Accounts:** Individual identities in AD for authentication and access control.  
- **Security Groups:** Collections of users for simplified permissions management.  
- **Group Policy Objects (GPOs):** Rules that can be applied to OUs to enforce policies.

---

## 🛠️ Configuration Details
- **Domain Name:** corp.local  
- **Domain Controller:** Windows Server 2019 (DC01)  
- **Static IP (DC01):** 192.168.10.10  
- **Clients:** Windows 10, Windows 11, Debian  
- **Example OU Structure:**  

---

## 🔹 Steps Performed

### 1️⃣ Create Organizational Units (OUs)
1. Open **Server Manager → Tools → Active Directory Users and Computers**.

![AD_Tools](images/1_AD_Users_Computers.png)

2. Right-click your domain (`corp.local`) → **New → Organizational Unit**.

![Create_OrgUnit](images/2_Org_Unit.png)


3. Enter the OU name (e.g., `LabUsers`) → **OK**
![LabUsers_OU](images/3_LabUsers.png)

4. Repeat to create sub-OUs like 'LabUsers →`IT` & `HR`, `LabComputers → Workstations` & `Servers`. `LabGroups`.

![IT_LabUsers](images/4_IT_LabUsers.png)

![HR_LabUsers](images/5_HR_LabUsers.png)
 
![Lab_Computers_OU](images/6_Lab_Computers_.png)

![LabComp_Workstations](images/7_LabComp_Workstations.png)

![LabComp_Servers](images/8_LabComp_Servers.png)

![LabGroup_OU](images/9_LabGroups.png)


5. Verify AD Users and Computers

![Verify_AD_OU](images/10_Verify_OU.png)


---

### 2️⃣ Create User Accounts

1. In **Active Directory Users and Computers**, expand your domain (`corp.local`) and navigate to the **OU where you want the user** (e.g., `LabUsers → IT` or `LabUsers → HR`) We chose HR this time.

![OU_Select](images/11_Select_OU.png)
   
2. Right-click the chosen OU (HR) → **New → User**.  
   
![New_User](images/12_New_User.png)

   
3. Enter user details:  
   - **First Name:** John  
   - **Last Name:** Doe  
   - **User Logon Name (UPN):** `JdoeHR@corp.local`
   - **User Logon Name (Pre-Windows 2000):** `CORP\JdoeHR`
   - **Click Next** 

![User_Details](images/13_Jdoe.png)

 
4. Set an **initial password**.  
   - Check **User must change password at next logon** (recommended for lab security).  
   - Optionally, uncheck if you want static passwords.

![Set_Password_Jdoe](images/14_Set_Password_Jdoe.png)

5. Review and Verify creation of John Doe user in HR
![Review_Jdoe_Users](images/15_Review_Jdoe_User.png)
![Verify_Users](images/16_Verify_Jdoe_HR.png)

6. Repeat for multiple accounts:

   - `HR` OU → Create users like `Eve HR`, `Charlie HR`
 
![New_User_EveHR](images/17_New_User_EveHR.png)
![New_User_CharlieHR](images/18_New_User_CharlieHR.png)

   
   - `IT` OU → Create users like `Alice IT`, `Bob IT`  

![New_User_AliceIT](images/19_New_User_AliceIT.png)
![New_User_BobIT](images/20_New_User_BobIT.png)
  
  
    
7. Verify that the accounts appear inside their respective OUs.  

- HR List
   ![Verify_Users_HR](images/21_Verify_Users_HR.png)


- IT List
   ![Verify_Users_IT](images/22_Verify_Users_IT.png)


---

### 3️⃣ Create Security Groups & Add Users

## 📌 Objective
Create security groups for users in the IT and HR OUs and assign users to their respective groups.  
This enables centralized management of permissions, policies, and resources in Active Directory.

---

## 🔹 Steps Performed

### 1. Decide on Security Groups
For this lab, create the following groups:

- `IT_Staff` → All users in the **IT OU**  
- `HR_Staff` → All users in the **HR OU**  

---

### 2. Create Security Groups
1. Open **Server Manager → Tools → Active Directory Users and Computers (ADUC)**.  
2. Navigate to the appropriate OU (e.g., **IT OU**).  
3. Right-click → **New → Group**.

![New_GroupIT](images/23_GroupIT.png)
  
4. Enter **Group name:** `IT_Staff`  
5. Set **Group scope:** Global  
6. Set **Group type:** Security → **OK**

![GroupIT_Config](images/24_GroupIT_Config.png)
 
7. Repeat for HR OU → create `HR_Staff`

   ![GroupHR_Config](images/25_GroupHR_Config.png)

8. Verify groups created (IT_Staff)(HR_Staff)

   ![Verify_IT_Staff](images/26_Verify_IT_Staff.png)
   
   ![Verify_HR_Staff](images/27_Verify_HR_Staff.png)

---

### 3. Add Users to Groups
1. Go to the **IT OU**, right-click **AliceIT** → **Add to a group…**  
   - Type `IT_Staff`, click **Check Names**, then **OK**

![Add_Alice_Group](images/28_Alice_Group.png)
![Add_Alice_IT_Staff](images/29_Alice_IT_Staff.png)
![Alice_Success_Joined](images/30_Alice_Success_Joined.png)


   
2. Go to the **HR OU**, right-click **EveHR** → **Add to a group…**  
  - Type `HR_Staff`, click **Check Names**, then **OK**

![Add_Eve_Group](images/31_Eve_Group.png)
![Add_Eve_HR_Staff](images/32_Eve_HR_Staff.png)
![Eve_Success_Joined](images/33_Eve_Success_Joined.png)


---


### 4. Verify Group Membership
1. Right-click the group (e.g., `IT_Staff`) → **Properties**  
2. Go to the **Members** tab to confirm users are listed  

![Verify_Group_HR](images/34_Verify_Group_HR.png)
![Verify_Group_IT](images/35_Verify_Group_IT.png)

---

✅ **Outcome:**  
- Users are logically grouped under **security groups**  
- Permissions and policies can now be applied at the group level instead of individually

---


# Step 4: Create and Link Group Policies (GPOs)

Now that users and groups are in place, we will create Group Policy Objects (GPOs) to manage settings for each OU.

---

## 1. Open Group Policy Management
- On your domain controller, open:
  **Server Manager → Tools → Group Policy Management**

---

## 2. Create a GPO for IT
1. Right-click the **IT OU** under `LabUsers → IT`.
2. Select **Create a GPO in this domain, and Link it here...**
3. Name it: 'IT_User_Policy'
4. Right-click the new GPO → **Edit**.
5. Example policies you can configure for IT users:
- Set a custom desktop background  
  (`User Configuration → Policies → Administrative Templates → Desktop → Desktop Wallpaper`)
- Disable Control Panel access  
  (`User Configuration → Administrative Templates → Control Panel → Prohibit access to Control Panel`)

---

## 3. Create a GPO for HR
1. Right-click the **HR OU** under `LabUsers → HR`.
2. Select **Create a GPO in this domain, and Link it here...**
3. Name it:  'HR_User_Policy'
4. Right-click the new GPO → **Edit**.
5. Example policies you can configure for HR users:
- Redirect Documents folder to a shared folder  
  (`User Configuration → Windows Settings → Folder Redirection`)
- Set password-protected screensaver  
  (`User Configuration → Administrative Templates → Control Panel → Personalization`)

---

## 4. Verify GPO Application
- Log in to a workstation as **Alice IT** or **Eve HR**.
- Run the command: 'gpupdate /force'

- Check that the GPO policies (wallpaper, restrictions, etc.) apply correctly.

---

✅ At this point, each OU has its own policies applied through GPOs


















### 5️⃣ Test Users and Groups
1. Log in from a client machine (Win 10/11 or Debian) using a domain user account.  
2. Confirm policies apply, and access permissions match the user’s group membership.  
3. Verify OU hierarchy and GPO links in **Active Directory Users and Computers** and **Group Policy Management Console**.

---

✅ **Outcome:**  
- Active Directory is structured with OUs for logical management.  
- Users and groups are created and organized.  
- GPOs applied to target OUs for policy enforcement.  
- Lab is ready for file server permissions, remote tools, and additional security configurations.
