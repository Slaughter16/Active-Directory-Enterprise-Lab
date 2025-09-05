# Step 6 – Implementing Security Policies

To enforce strong password standards across the domain, I configured the **Default Domain Policy** in Group Policy Management

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

## Account Lockout Policy Configuration

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

### Configured Settings (Best Practice)
- **Account Lockout Threshold:** 3 invalid logon attempts  
- **Account Lockout Duration:** 30 minutes  '
- **Allow Administrator account lockout:** Disabled
- **Reset Account Lockout Counter After:** 30 minutes
 ![Config_Lockout_Policy](images/16_Config_Lockout_Policy.png)
---

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

## User Rights Assignment Configuration

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
- On the **Windows 10 client (EveHR)**:  
  - Attempted RDP with `HR_Staff` → **Access Denied**.  
  - Attempted RDP with `IT_Staff` → **Access Granted** ✅.  

---

✅ This ensures only authorized IT staff can use RDP for administrative tasks, while HR staff have restricted access, reducing the attack surface and following the principle of least privilege.
