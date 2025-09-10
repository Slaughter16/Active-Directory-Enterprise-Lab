# Step-8_Effective_Permissions_and_Inheritance

## 📌 Objective  
Demonstrate **explicit permissions, inheritance, group memberships, and deny permissions** using NTFS and file sharing.  

We will create a **Common** folder accessible to everyone, but restrict the **Project** subfolder so only members of the `Project` security group have access. The **Events** subfolder will remain fully accessible to all.  

---

## 🛠️ Steps  

### 1️⃣ Create Security Group in ADUC  
1. On **Windows Server**, open **Active Directory Users and Computers (ADUC)**.  
2. Navigate to `LabUsers` OU → **New → Group**.  
   - **Group name:** `Project`  
   - **Group scope:** Global  
   - **Group type:** Security  
3. Add users (e.g., Alice, Bob) as members of the `Project` group.  

📸 *[Insert screenshot: ADUC with Project group creation]*  

---

### 2️⃣ Create Folder Structure  
1. On the server, open **File Explorer** → `C:\`.  
2. Create a new folder named **Common**.  
3. Inside **Common**, create two subfolders:  
   - `Project`  
   - `Events`  
📸 *[Insert screenshot: Folder structure in C:\]*  

---

### 3️⃣ Configure Share Permissions  
1. Right-click **Common** → **Properties** → **Sharing** → **Advanced Sharing**.  
2. Check **Share this folder** → click **Permissions**.  
3. Add `Everyone` → assign **Full Control**.  

📸 *[Insert screenshot: Share permissions for Everyone]*  

---

### 4️⃣ Configure NTFS Permissions (Inheritance)  
1. On **Common** folder → **Properties → Security** tab.  
2. Edit → add `Everyone` → assign **Full Control**.  
   - This ensures permissions flow to all subfolders (`Project`, `Events`, `Contracts`).  
3. Check that **Project** and **Events** inherited the same permissions.  
4. (Optional) Inside `Project`, create another subfolder `Contracts` to verify inheritance behavior.
📸 *[Insert screenshot: NTFS permissions for Common]*  

---

### 5️⃣ Restrict the Project Subfolder  
1. Right-click `Project` → **Properties → Security → Advanced**.  
2. Click **Disable inheritance**.  
3. Choose **Convert inherited permissions into explicit permissions**.  
4. Remove `Everyone`.  
5. Add the `Project` security group → assign **Modify** (or **Full Control**).  

📸 *[Insert screenshot: Project subfolder advanced permissions]*  

---

### 6️⃣ Verify Permissions  
- **Login as a Project group member (e.g., AliceIT):**  
  - Should access `Common`, `Events`, and `Project`.  
- **Login as a non-Project user (e.g., EveHR):**  
  - Should access `Common` and `Events`.  
  - Should be denied access to `Project`.  

📸 *[Insert screenshot: Access test results]*  

---

## ✅ Results  
- `Common` → accessible by everyone.  
- `Events` → accessible by everyone (inherits from Common).  
- `Project` → restricted, only `Project` group members can access.  
- Inheritance successfully disabled and overridden for `Project`.  
