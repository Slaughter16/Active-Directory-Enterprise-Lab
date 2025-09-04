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
