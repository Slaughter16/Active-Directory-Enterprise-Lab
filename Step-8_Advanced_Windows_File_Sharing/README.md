# Step-8_Effective_Permissions_and_Inheritance

## 📌 Objective  
Demonstrate **explicit permissions, inheritance, group memberships, and deny permissions** using NTFS and file sharing.  

We will create a **Common** folder accessible to everyone, but restrict the **Project** subfolder so only members of the `Project` security group have access. The **Events** subfolder will remain fully accessible to all.  

---

## 🛠️ Steps  

### 1️⃣ Create Security Group in ADUC  
1. On **Windows Server**, open **Active Directory Users and Computers (ADUC)**.
![Tools_ADUC](images/1_Tools_ADUC.png)
 
2. Navigate to `LabUsers` OU → **New → Group**.  
   - **Group name:** `Project`  
   - **Group scope:** Global  
   - **Group type:** Security
![New_Group_Project](images/2_New_Group_Project.png)
![Group_Name_Project](images/3_Group_Name_Project.png)
3. Add user (Alice) as member of the `Project` group.  
![Add_Alice_Group](images/4_Add_Alice_Group.png)
![Select_Project_Group](images/5_Select_Project_Group.png)
![Verify_Alice_Added_Group](images/6_Verify_Alice_Added_Group.png)


---

### 2️⃣ Create Folder Structure  
1. On the server, open **File Explorer** → `C:\`.
![Goto_Local_Disk](image/7_Goto_Local_Disk.png)  
2. Create a new folder named **Common**.
![Create_Common_Folder](images/8_Create_Common_Folder.png)
3. Inside **Common**, create two subfolders:  
   - `Project`  
   - `Events`  
![Create_Subfolders](images/9_Create_Subfolders.png)
---

### 3️⃣ Configure Share Permissions  
1. Right-click **Common** → **Properties** → **Sharing** → **Share** → **Add Everyone** → **Share**.
 ![Common_Properties](images/10_Common_Properties.png)
 ![Share_Common_Prop](images/11_Share_Common_Prop.png)
 ![Share_Common_Folder](images/13_Share_Common_Folder.png)
 ![Confirm_Folder_Shared](images/14_Confirm_Folder_Shared.png)


2.**Common** → **Properties** → **Sharing** → **Advanced Sharing** → **Permissions** → **Allow Full Control** → **Apply** 
![Advanced_Sharing_Common](images/15_Advanced_Sharing_Common.png)
3. Check **Share this folder** → click **Permissions**. 
![Permissions_AdvSharing](images/16_Permissions_AdvSharing.png)
4. Add `Everyone` → assign **Full Control**.  
![Allow_FullControl_Everyone](images/17_Allow_FullControl_Everyone.png)

---

### 4️⃣ Configure NTFS Permissions (Inheritance)  
1. On **Common** folder → **Properties → Security** tab.
![Security_Permissions_Common_Everyone](images/18_Security_Permissions_Common_Everyone.png)
2. Edit → add `Everyone` → assign **Full Control**.  
   - This ensures permissions flow to all subfolders (`Project`, `Events`, `Contracts`).
 ![Verify_Security_Everyone_Perm](images/19_Verify_Security_Everyone_Perm.png)  
4. Check that **Project** and **Events** inherited the same permissions.
![Events_Prop](images/20_Events_Prop.png)
![Verify_Events_Everyone_Share](images/21_Verify_Events_Everyone_Share.png)
![Security_Everyone_Permission_Events](images/22_Security_Everyone_Permission_Events.png)

![Project_Prop](images/23_Project_Prop.png)
![Project_Everyone_Share](images/24_Project_Everyone_Share.png)
![Verify_Security_Everyone_Permission_Project](images/25_Verify_Security_Everyone_Permission_Project.png)
6. (Optional) Inside `Project`, create another subfolder `Contracts` to verify inheritance behavior.
![Contract_Prop](images/26_Contract_Prop.png)
![Contract_Prop_Everyone](images/27_Contract_Prop_Everyone.png)
![Security_Everyone_Contract](images/28_Security_Everyone_Contract.png)


---

### 5️⃣ Restrict the Project Subfolder  
1. Right-click `Project` → **Properties → Security → Advanced**.
![Project_Properties](images/29_Project_Properties.png)
![Project_Security_Advanced](images/30_Project_Security_Advanced.png)
2. Click **Disable inheritance**.  
3. Choose **Convert inherited permissions into explicit permissions**.
![Disable_Inheritance_Convert](images/31_Disable_Inheritance_Convert.png)  
4. Remove `Everyone`.
![Remove_Everyone](images/32_Remove_Everyone.png)
5. Add the `Project` security group → assign **Modify** (or **Full Control**).  
![Add_Project](images/33_Add_Project.png)
![Select_Project_Permission](images/34_Select_Project_Permission.png)
![Allow_FullControl_Project](images/35_Allow_FullControl_Project.png)

6. Verify Project permission added and Verify permissions of Project in Security tab.
![Click_Ok_Project_Permissions](images/36_Click_Ok_Project_Permissions.png)
![Verify_Project_Security_Permissions](images/37_Verify_Project_Security_Permissions.png)
---

### 6️⃣ Verify Permissions  
- **Login as a Project group member (AliceIT, WIN11):**  
  - Should access `Common`, `Events`, and `Project`.
 ![WIN_SERVER_COMMMON_WIN11](images/38_WIN_SERVER_COMMMON_WIN11.png)
![Common_Events_Project_WIN11](images/39_Common_Events_Project_WIN11.png)
![Events_WIN11](images/40_Events_WIN11.png)
![Project_ContractsFolder_WIN11](images/41_Project_ContractsFolder_WIN11.png)
![Project_Contracts_WIN11](images/42_Project_Contracts_WIN11.png)
- **Login as a non-Project user (e.g., EveHR):**  
  - Should access `Common` and `Events`.
 ![WIN10_WIN_SERVER_COMMMON](images/43_WIN10_WIN_SERVER_COMMMON.png)
- Only Events is visible, since EveHR is not in the Project group. This demonstrates the restriction.
![WIN10_Common_Events](images/44_WIN10_Common_Events.png)

---

## ✅ Results  
- `Common` → accessible by everyone.  
- `Events` → accessible by everyone (inherits from Common).  
- `Project` → restricted, only `Project` group members can access.  
- Inheritance successfully disabled and overridden for `Project`.  





# Explicit Deny Permissions in NTFS

In this step, we configure **explicit deny permissions** in NTFS to restrict access for a single user (EveHR) to a confidential folder, while allowing all other employees access.

---

## Scenario
- A **Project** folder is shared with the **AllEmployees** group.  
- The folder has two subfolders:  
  - `Confidential` → should be hidden from or inaccessible to **EveHR**.  
  - `Materials` → should remain accessible to all employees.  

EveHR is a member of the **AllEmployees** security group, but we need to block her explicitly from accessing the **Confidential** folder without impacting other users.

---

## Steps

### 1. Create Folder Structure
1. On the server, open **File Explorer**.  
2. Create the following folders on `C:\`:  
   - `Project`  
   - Inside `Project`, create:
     - `Confidential`  
     - `Materials`  

📸 *Screenshot: Folder structure in File Explorer*

---

### 2. Configure Share Permissions
1. Right-click **Project → Properties → Sharing → Advanced Sharing**.  
2. Enable **Share this folder**.  
3. Add **Everyone** with **Full Control**.  
   - (Share permissions are typically left open; NTFS handles security restrictions.)  

📸 *Screenshot: Advanced Sharing window*

---

### 3. Configure NTFS Permissions
1. Right-click the **Project** folder → **Properties → Security → Edit**.  
2. Ensure **Everyone** has **Read/Write (Modify)** permissions.  
3. Inherit permissions down to both subfolders.

📸 *Screenshot: Project folder permissions*

---

### 4. Apply Explicit Deny for EveHR
1. Go to **Confidential → Properties → Security → Advanced**.  
2. **Disable inheritance** → *Convert existing permissions to explicit*.  
3. Remove unnecessary inherited groups if needed (keep Admins/System).  
4. Add **EveHR**:  
   - **Allow** → *This folder only* → ✔️ List folder / read data  
   - **Deny** → *This folder only* → ❌ Read & execute, ❌ Read  

This ensures EveHR can **see** the `Confidential` folder but will get **Access Denied** when attempting to open it.

📸 *Screenshot: Advanced Security settings for Confidential*

---

### 5. Testing
- **Log in as Alice (Project member):**  
  - Can access `Project → Confidential` and `Materials`.  
- **Log in as EveHR (HR staff):**  
  - Can see `Project → Materials` normally.  
  - Can **see the `Confidential` folder**, but receives **Access Denied** when attempting to open it.

📸 *Screenshot: Access Denied message for EveHR*  

---

## Key Notes
- **Share permissions** = broad access (Everyone: Full).  
- **NTFS permissions** = precise control (used to enforce restrictions).  
- **Explicit deny** always overrides allow.  
- Allowing “List folder / read data” ensures the folder is visible, while denying read/execute prevents entry.  

---

✅ At the end of this step:  
- All employees except EveHR can access both subfolders.  
- EveHR sees the Confidential folder but cannot access it.
