# Step 6 – Implementing Security Policies

## 📑 Table of Contents
🎯 [Objectives](#objectives)
1. 📝 [Password Policy Configuration](#password-policy-configuration)
   - [Configured Settings](#configured-settings)
   - [Testing & Validation](#testing--validation)
2. 🔒 [Account Lockout Policy Configuration](#account-lockout-policy-configuration)
   - [Configured Settings](#configured-settings-lockout)
   - [Testing & Validation](#testing--validation-lockout)
3. 👥 [User Rights Assignment Configuration](#user-rights-assignment-configuration)
   - [Configured Settings](#configured-settings-user-rights)
   - [Testing & Validation](#testing--validation-user-rights)
4. 🔐 [Implement Fine-Grained Password Policies (FGPP)](#implement-fine-grained-password-policies-fgpp)
   - [Administrative Accounts](#administrative-accounts)
   - [Standard Users](#standard-users)
   - [Verification](#verification-fgpp)

---
<a id="objectives"></a>
## 🎯 Objectives
To implement domain-wide password and account security policies using Group Policy, including strong password enforcement, account lockout thresholds, and role-based access control, thereby strengthening security posture and mitigating potential credential-based threats.

---

## Password Policy Configuration

### Steps
1. Open **Group Policy Management Console (GPMC)**.  
   ![Open_GPM](images/1_Open_GPM.png)

2. Navigate to **Group Policy Objects**, right-click **Default Domain Policy**, and select **Edit**.  
   ![Edit_Default_Domain_Policy](images/2_Edit_Default_Domain_Policy.png)

3. In the Group Policy Management Editor, go to:  
   `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy`  
   ![Before_Password_Policy_Config](images/3_Password_Policy_Before.png)

---

### Configured Settings:
- **Enforce Password History:** 24 passwords remembered  
- **Maximum Password Age:** 60 days  
- **Minimum Password Age:** 1 day  
- **Minimum Password Length:** 12 characters  
- **Password Must Meet Complexity Requirements:** Enabled  

![After_Password_Policy_Config](images/4_Password_Policy_After.png)
### Why These Settings?
- **Password History** prevents users from recycling recent passwords.  
- **Maximum Age** forces regular password changes.  
- **Minimum Age** stops users from cycling through multiple changes in one sitting.  
- **Minimum Length + Complexity** enforces stronger, harder-to-crack passwords.  

---

### Testing & Validation

- Forced a password reset for **AliceIT** in **Active Directory Users and Computers (ADUC)**  
  *(Right-click user → Reset Password → check “User must change password at next logon”).*  
![ADUC_AliceIT](images/5_ADUC_AliceIT.png)
![Reset_Password_AliceIT](images/6_Reset_Password_AliceIT.png)
![Reset_Password_AliceIT2](images/7_Reset_Password_AliceIT2.png)


- On the **WIN-11 client (AliceIT)** machine, logged in as a domain user.
   ![Alice_Password_Change](images/8_Alice_Password_Change.png)

- Attempted to set a weak password (e.g., `password1`) → **Rejected**. ✅

     ![Weak_Password](images/9_Weak_Password.png)
   ![Unable_Set_Weak_Password](images/10_Unable_Set_Weak_Password.png)

- Set a strong password (e.g., `P@ssw0rd!2025`) → **Accepted**. ✅
   ![Strong_Password](images/11_Strong_Password.png)
   ![Alice_Password_Change_Success](images/12_Alice_Password_Change_Success.png)


---

✅ With this configuration and testing, all Active Directory users in the **corp.local** domain are now required to follow a strong password policy aligned with enterprise best practices.

---
<a id="account-lockout-policy-configuration"></a>
## 🔒 Account Lockout Policy Configuration

To protect against brute-force attacks, I configured an **Account Lockout Policy** in the Default Domain Policy. This ensures that repeated invalid login attempts will temporarily lock the user’s account.

---
### Steps
1. Open **Group Policy Management Console (GPMC)**.  
   ![GPMC_Acct_Policy](images/13_GPMC_Acct_Policy.png)
2. Navigate to **Group Policy Objects**, right-click **Default Domain Policy**, and select **Edit**.  
   ![Default_Domain_Acct_Policy](images/14_Default_Domain_Acct_Policy.png)

3. In the Group Policy Management Editor, go to:  
   `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy`  
   ![Account_Lockout_Settings](images/15_Account_Lockout_Settings.png)

   
---
<a id="configured-settings"></a>
### Configured Settings (Best Practice)
- **Account Lockout Threshold:** 3 invalid logon attempts  
- **Account Lockout Duration:** 30 minutes  '
- **Allow Administrator account lockout:** Disabled
- **Reset Account Lockout Counter After:** 30 minutes
 ![Config_Lockout_Policy](images/16_Config_Lockout_Policy.png)
---
<a id="testing--validation"></a>
### Testing & Validation
- On the **Windows 10 client (EveHR)**, attempted to log in with the wrong password **3 times in a row**.  
- After the 3rd failed attempt → **Account locked out** ✅  
- Verified lockout message on login screen:  
  *“Your account has been locked. Please contact your administrator or try again later.”*  
- Account automatically unlocked after the configured **30 minutes**.  
![Verify_WIN10_Lockout](images/17_Verify_WIN10_Lockout.png)
---

✅ With this configuration, repeated brute-force password attempts against domain accounts are mitigated by temporary account lockouts, reducing the risk of credential-guessing attacks.

---
<a id="user-rights-assignment"></a>
## 👥 User Rights Assignment Configuration

To enforce **role-based access control (RBAC)** and enhance security, I configured **User Rights Assignment** in the **Default Domain Policy**.

---

### Steps
1. Open **Group Policy Management Console (GPMC)**.  
![GPMC_RBAC](images/18_GPMC_RBAC.png)
2. Navigate to **Group Policy Objects**, right-click **Default Domain Policy**, and select **Edit**.  
![Edit_Default_RBAC](images/19_Edit_Default_RBAC.png)
3. In the Group Policy Management Editor, go to:  
   `Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → User Rights Assignment`  
![User_Rights_Access](images/20_User_Rights_Access.png)
---

### Configured Settings

#### 🔒 Deny Log on Locally
- **Policy:** Deny log on locally  
- **Applied to:** `HR_Staff` group  
- **Purpose:** Prevent HR users from logging on directly to servers or domain controllers.  

![HR_Staff_Deny](images/21_HR_Staff_Deny.png)
![HR_Staff_Deny_Add](images/22_HR_Staff_Deny_Add.png)
![HR_Staff_Deny_Add_Verify](images/23_HR_Staff_Deny_Add_Verify.png)


#### 💻 Allow Log on Through Remote Desktop Services
- **Policy:** Allow log on through Remote Desktop Services  
- **Applied to:** `IT_Staff` group  
- **Purpose:** Restrict remote desktop access to IT administrators only.  
![IT_Staff_Allow](images/24_IT_Staff_Allow.png)
![IT_Staff_Allow_Add](images/25_IT_Staff_Allow_Add.png)
![IT_Staff_Allow_Add_Verify](images/26_IT_Staff_Allow_Add_Verify.png)

---

### Testing & Validation
- On the **Windows 11 client (AliceIT)**:  
  - Confirmed that an **HR_Staff** account cannot log in locally.
![HR_Staff_Login_WINServer](images/27_HR_Staff_Login_WINServer.png)
![HR_Staff_Denied_WINServer](images/28_HR_Staff_Denied_WINServer.png) 
- On the **Windows 10 client (EveHR)**:  
  - Attempted RDP with `HR_Staff` → **Access Denied**.
![RDP_HR_Connect](images/29_RDP_HR_Connect.png)
![RDP_HR_Password](images/30_RDP_HR_Password.png) 
![RDP_HR_Connect_Denied](images/31_RDP_HR_Connect_Denied.png) 


  - Attempted RDP with `IT_Staff` → **Access Granted** ✅.  
![RDP_HR_Connect_Denied](images/32_RDP_IT_Connect.png) 
![RDP_HR_Connect_Denied](images/33_RDP_IT_Password.png) 
![RDP_HR_Connect_Denied](images/34_RDP_IT_Connect_Accepted.png) 

---

✅ This ensures only authorized IT staff can use RDP for administrative tasks, while HR staff have restricted access, reducing the attack surface and following the principle of least privilege.

---


# 🔐 Step 6 – Implement Fine-Grained Password Policies (FGPP)

## 📌 Scenario
The organization wants stricter password rules for administrative accounts, while standard users follow less stringent requirements.  
We’ll use **Password Settings Objects (PSOs)** in **Active Directory Administrative Center (ADAC)** to apply different password policies to different groups.

---

## 🛠️ Configuration

### 1️⃣ Open ADAC
1. On the **Windows Server (Domain Controller)**, open:  
   **Server Manager → Tools → Active Directory Administrative Center (ADAC)**

![Open_ADAC](images/35_Open_ADAC.png)
3. Navigate to:  
   `corp.local → System → Password Settings Container`  
![Passwords_Settings_Container](images/36_Passwords_Settings_Container.png)

---

### 2️⃣ Create a Policy for Administrative Accounts
1. In the right-hand pane, click **New → Password Settings**.

![Password_Settings_New_Admin](images/37_Password_Settings_New_Admin.png)

3. Configure the following:  
   - **Name:** `AdminPasswordPolicy`  
   - **Precedence:** `1` (lowest number = highest priority)  
   - **Minimum Password Length:** `15`  
   - **Enforce Password History:** `5`  
   - **Complexity Requirements:** Enabled  
4. Under **Directly Applies To → Add**, select the **IT_Staff** group.

   ![Admin_Password_Policy](images/38_Admin_Password_Policy.png)

6. Click **OK** to create the policy.
![IT_Staff_Add](images/39_IT_Staff_Add.png)

✅ Outcome: All IT staff (admins) must use strong 15+ character passwords with history enforcement.

---

### 3️⃣ Create a Policy for Standard Users
1. In the same container, click **New → Password Settings** again.
![Verify_Admin_Pass_Policy](images/40_Verify_Admin_Pass_Policy.png)

3. Configure the following:  
   - **Name:** `UserPasswordPolicy`  
   - **Precedence:** `2`  
   - **Minimum Password Length:** `10`  
   - **Enforce Password History:** `5`  
   - **Complexity Requirements:** Enabled
![User_Password_Policy](images/41_User_Password_Policy.png)

4. Under **Directly Applies To → Add**, select the **HR_Staff** group.
6. Click **OK** to create the policy.
![HR_Staff_Add](images/42_HR_Staff_Add.png)

✅ Outcome: HR users must use at least 10-character passwords with complexity.

---

### 4️⃣ Verification
1. Run `ADAC` → check **Password Settings Container** → ensure both PSOs are listed.
![Verify_User_Password_Policy](images/43_Verify_User_Password_Policy.png)

2. On WIN-Server, confirm applied policies:

```powershell
Get-ADUserResultantPasswordPolicy -Identity AliceIT
Get-ADUserResultantPasswordPolicy -Identity EveHR
```
![Power_Shell_Verify](images/44_Power_Shell_Verify.png)


3. Test on Server (Resetting Passwords in ADUC)
- Tried resetting passwords for both users directly in Active Directory Users and Computers (ADUC).

#### EveHR (HR Staff – requires 10 chars)


- Attempted short password → ❌ Failed (did not meet requirements).

![Reset_Eve_Pass_Fail](images/45_Reset_Eve_Pass_Fail.png)

![New_Pass_Eve_Fail](images/46_New_Pass_Eve_Fail.png)

![Verify_Pass_Fail_Eve](images/47_Verify_Pass_Fail_Eve.png)


  
- Attempted strong 10+ char password → ✅ Success.
![Eve_Correct_Pass](images/53_Eve_Correct_Pass.png)
![Verify_Pass_Eve_Change](images/54_Verify_Pass_Eve_Change.png)


#### AliceIT (IT Staff – requires 15 chars)


- Attempted short password → ❌ Failed (did not meet requirements).
![Reset_Alice_Pass_Fail](images/48_Reset_Alice_Pass_Fail.png)
![New_Pass_Alice_Fail](images/49_New_Pass_Alice_Fail.png)
![Verify_Pass_Fail_Alice](images/50_Verify_Pass_Fail_Alice.png)


- Attempted strong 15+ char password → ✅ Success.
![Alice_Correct_Pass](images/51_Alice_Correct_Pass.png)
![Verify_Pass_Alice_Change](images/52_Verify_Pass_Alice_Change.png)






4. On a client machine (e.g., Windows 10/11), try resetting a password for a user in **IT_Staff** vs. **HR_Staff**.

- Windows 10, HR user (EveHR) → must set 10+ character password.  
 - First entry doesnt meet requirements, second entry will.
  ![Change_Eve_Pass_Sign](images/55_Change_Eve_Pass_Sign.png)
  ![Fail_Pass_Change_Eve](images/56_Fail_Pass_Change_Eve.png)
  ![Unable_Update_Pass_Eve](images/57_Unable_Update_Pass_Eve.png)
  ![Pass_Change_Eve](images/58_Pass_Change_Eve.png)
  ![Eve_Pass_Change](images/59_Eve_Pass_Change.png)


- Windows 11, IT user (AliceIT) → must set 15+ character password.

![Alice_Pass_Change_Sign](images/60_Alice_Pass_Change_Sign.png)
![Fail_Pass_Change_Alice](images/61_Fail_Pass_Change_Alice.png)
![Alice_Unable_UpdatePass](images/62_Alice_Unable_UpdatePass.png)
![Alice_Pass_Change](images/63_Alice_Pass_Change.png)






















