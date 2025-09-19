# Step-8_Effective_Permissions_and_Inheritance

## 📌 Objective  
Demonstrate **explicit permissions, inheritance, group memberships, and deny permissions** using NTFS and file sharing.  

- **Common** folder → accessible to everyone.  
- **Project** subfolder → restricted to `Project` security group members.  
- **Events** subfolder → fully accessible to all.
---

## 📑 Table of Contents  
- <a href="#create-security-group">1️⃣ Create Security Group in ADUC</a>  
- <a href="#create-folder-structure">2️⃣ Create Folder Structure</a>  
- <a href="#share-permissions">3️⃣ Configure Share Permissions</a>  
- <a href="#ntfs-permissions">4️⃣ Configure NTFS Permissions (Inheritance)</a>  
- <a href="#restrict-project">5️⃣ Restrict the Project Subfolder</a>  
- <a href="#verify-permissions">6️⃣ Verify Permissions</a>  
- <a href="#contracts-subfolder">📂 Separate Share for Contracts Subfolder</a>  
- <a href="#explicit-deny">🚫 Explicit Deny Permissions in NTFS</a>  

---

## 🛠️ Steps  
<a id="create-security-group"></a>
### 1️⃣ Create Security Group in ADUC  
1. Open **Active Directory Users and Computers (ADUC)** on Windows Server.  
![Tools_ADUC](images/1_Tools_ADUC.png)
 
2. Navigate to `LabUsers` OU → **New → Group**.  
   - **Group name:** `Project`  
   - **Group scope:** Global  
   - **Group type:** Security
![New_Group_Project](images/2_New_Group_Project.png)
![Group_Name_Project](images/3_Group_Name_Project.png)
3. Add a user (Alice) as member to the `Project` group.  
![Add_Alice_Group](images/4_Add_Alice_Group.png)
![Select_Project_Group](images/5_Select_Project_Group.png)
![Verify_Alice_Added_Group](images/6_Verify_Alice_Added_Group.png)


---
<a id="create-folder-structure"></a>
### 2️⃣ Create Folder Structure  
1. On the server, open **File Explorer** → `C:\`and Create a new folder named **Common**.
![Create_Common_Folder](images/8_Create_Common_Folder.png)
2. Inside **Common**, create two subfolders:  
   - `Project`  
   - `Events`  
![Create_Subfolders](images/9_Create_Subfolders.png)

---
<a id="share-permissions"></a>
### 3️⃣ Configure Share Permissions  
1. Right-click **Common** → **Properties** → **Sharing** → **Share** → **Add Everyone** → **Share**.
 ![Common_Properties](images/10_Common_Properties.png)
 ![Share_Common_Prop](images/11_Share_Common_Prop.png)
 ![Share_Common_Folder](images/13_Share_Common_Folder.png)
 ![Confirm_Folder_Shared](images/14_Confirm_Folder_Shared.png)


2. Click **Advanced Sharing → Permissions → Allow Full Control → Apply**.  
![Advanced_Sharing_Common](images/15_Advanced_Sharing_Common.png)
3. Check **Share this folder** → click **Permissions**. 
![Permissions_AdvSharing](images/16_Permissions_AdvSharing.png)
4. Add `Everyone` → assign **Full Control**.  
![Allow_FullControl_Everyone](images/17_Allow_FullControl_Everyone.png)

---
<a id="ntfs-permissions"></a>
### 4️⃣ Configure NTFS Permissions (Inheritance)  
1. Right-click **Common** → **Properties → Security**.  
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
<a id="restrict-project"></a>
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
<a id="verify-permissions"></a>
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

## ✅ Results  
- `Common` → accessible by everyone.  
- `Events` → accessible by everyone (inherits from Common).  
- `Project` → restricted, only `Project` group members can access.  
- Inheritance successfully disabled and overridden for `Project`.  

---

<a id="contracts-subfolder"></a>
# 📂 Separate Share for Contracts Subfolder

## 📌 Objective
Allow **EveHR** to access the `Contracts` folder without granting access to the parent `Project` folder, while keeping full control for the `Project` group. Demonstrates **explicit permissions, inheritance, and NTFS security**.

---
## 1️⃣ Folder Structure
- C:\Common
 - ├─ Project # Restricted to Project group
 - └─ Contracts # Separate share, accessible to EveHR
 
- **Project** → only `Project` group can access.  
- **Contracts** → Project members + EveHR can access.


---

## 2️⃣ Share Permissions

### Contracts Folder
1. Right-click `Contracts` → **Properties → Sharing → Advanced Sharing**.
![Contracts_Advanced_Sharing](images/62_Contracts_Advanced_Sharing.png)
3. Check **Share this folder**.  
4. Click **Permissions** → configure:  

| Group/User | Permission       |
|------------|----------------|
| Project    | Full Control    |
| EveHR      | Read / Change    |
| Everyone   | Remove (optional) |
![Contracts_Eve_Permissions](images/63_Contracts_Eve_Permissions.png)
![Contracts_Project_Permissions](images/64_Contracts_Project_Permissions.png)
4. Apply changes.
![Contracts_Adv_Sharing_Folder_Check](images/65_Contracts_Adv_Sharing_Folder_Check.png)
> Share permissions can be broad, but NTFS will enforce strict access.


---

## 3️⃣ NTFS Permissions

### Configure Explicit Permissions
1. Right-click `Contracts` → **Properties → Security → Advanced**.  
2. Click **Disable inheritance** → **Convert inherited permissions to explicit**.  
3. Remove any groups/users that should **not** have access.  
4. Add/verify permissions:

| Group/User | Permissions                          | Applies to                        |
|------------|--------------------------------------|----------------------------------|
| Project    | Full Control                          | This folder, subfolders, files   |
| EveHR      | Read / Write / Execute (or appropriate access) | This folder, subfolders, files   |

![Contracts_DisableInher_Add_Eve](images/66_Contracts_DisableInher_Add_Eve.png)

---

### Project Folder
- Keep **Everyone** or non-HR users **removed**.  
- Project group retains **Full Control**.  
- EveHR will **not see** or access Project folder.

---
## 4️⃣ Test Access

| User      | Path                     | Expected Result                     |
|-----------|-------------------------|------------------------------------|
| EveHR     | `\\Server\Contracts`    | Can open & modify files             |
| EveHR     | `\\Server\Project`      | Denied / folder not visible         |
| Project   | `\\Server\Project`      | Full access                         |
| Project   | `\\Server\Contracts`    | Full access                         |

- EveHR able to open Contracts Folder
![Eve_Contracts_Run](images/67_Eve_Contracts_Run.png)
![Eve_Contracts_Open](images/68_Eve_Contracts_Open.png)

- Project member (AliceIT)
![Alice_Project_Open](images/69_Alice_Project_Open.png)
![Alice_Project_Contracts_Open](images/70_Alice_Project_Contracts_Open.png)

---

## ✅ Outcome

- EveHR can access the `Contracts` subfolder **without seeing Project**.  
- Project group retains full control over Project and Contracts.  
- Demonstrates **NTFS explicit permissions, inheritance, and targeted access control**.

---

## 💡 Notes
- Always apply NTFS permissions **after** share permissions.  
- Explicit deny is not used here; EveHR access is granted **only on Contracts** via a separate share.  
- Inheritance is disabled on Contracts to prevent unwanted permission propagation from Project folder.

---

<a id="explicit-deny"></a>
# 🚫 Explicit Deny Permissions in NTFS

## 📌 Objective  
In this step, we configure **explicit deny permissions** in NTFS to restrict access for a single user (EveHR) to a confidential folder, while allowing all other employees access.

---

## Scenario
- A **Project** folder is shared with the **Everyone** group.  
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
![Create_Project_Folder](images/45_Create_Project_Folder.png)
   - Inside `Project`, create:
     - `Confidential`  
     - `Materials`  
![Create_Subfolder_Conf_Mat](images/46_Create_Subfolder_Conf_Mat.png)

---

### 2. Configure Share Permissions
1. Right-click **Project → Properties → Sharing → Advanced Sharing**.
![Project_Advanced_Sharing](images/47_Project_Advanced_Sharing.png)
3. Enable **Share this folder**.  
4. Add **Everyone** with **Full Control**.  
   - (Share permissions are typically left open; NTFS handles security restrictions.)  

![Project_Everyone_Permissions](images/48_Project_Everyone_Permissions.png)
![Verify_Share_Project_Folder](images/49_Verify_Share_Project_Folder.png)

---

### 3. Configure NTFS Permissions
1. Right-click the **Project** folder → **Properties → Security → Edit**.
 ![Project_Security_Edit_Permission](images/50_Project_Security_Edit_Permission.png)  
3. Ensure **Everyone** has **Read/Write (Modify)** permissions.
![Add_Everyone_Project_Permission](images/51_Add_Everyone_Project_Permission.png)

5. Inherit permissions down to both subfolders.

📸 *Screenshot: Project folder permissions*

---

### 4. Apply Explicit Deny for EveHR
1. Go to **Confidential → Properties → Security → Advanced**.
![Confidential_Prop](images/52_Confidential_Prop.png)
5. Add **EveHR**:  
   - **Deny** → ❌ Read  
![Add_Evehr_Deny_Read_Permission](images/53_Add_Evehr_Deny_Read_Permission.png)
3. Click **Yes** for Deny permission
![Confirm_Deny_Permission](images/54_Confirm_Deny_Permission.png)
This ensures EveHR can **see** the `Confidential` folder but will get **Access Denied** when attempting to open it.

📸 *Screenshot: Advanced Security settings for Confidential*

---

### 5. Testing
- **Log in as Alice (Project member):**  
  - Can access `Project → Confidential` and `Materials`.
![Alice_Project_Folder](images/55_Alice_Project_Folder.png)
![Alice_Open_Confidential_Success](images/56_Alice_Open_Confidential_Success.png)
![Alice_Open_Materials_Success](images/57_Alice_Open_Materials_Success.png)
- **Log in as EveHR (HR staff):**  
  - Can see `Project → Materials` normally.
  ![Evehr_Open_Project](images/58_Evehr_Open_Project.png)
![Evehr_Materials_Access_Granted](images/59_Evehr_Materials_Access_Granted.png)
  - Can **see the `Confidential` folder**, but receives **Access Denied** when attempting to open it.
![Attempt_Confidential_Folder_Access](images/60_Attempt_Confidential_Folder_Access.png)
![Evehr_Deny_Confidential_Access](images/61_Evehr_Deny_Confidential_Access.png)

---

## Key Notes
- **Share permissions** = broad access (Everyone: Full).  
- **NTFS permissions** = precise control (used to enforce restrictions).  
- **Explicit deny** overrides allow.  
- Allowing “List folder / read data” ensures the folder is visible, while denying read/execute prevents entry.  

---

✅ At the end of this step:  
- All employees except EveHR can access both subfolders.  
- EveHR sees the Confidential folder but cannot access it.
