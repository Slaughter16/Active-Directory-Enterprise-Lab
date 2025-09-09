# 🔧 Step 7 – Implementing Service Accounts & Kiosk Setup

## 📌 Objective
Set up a **single-purpose computer** to automatically log in with a **service account**, launch a specific web page in full-screen mode, and restrict local logon for all standard users. This simulates a kiosk-style workstation without using Windows Kiosk mode.

**Prerequisites:**  
- Windows Server with **AD DS installed**  
- Windows 10 Pro / Enterprise client  
- **Sysinternals Suite**  
- AD security groups set up (e.g., `LabUsers`)  
- Web browser (e.g., Google Chrome)

---

- ### 1️⃣ Create a Service Account in Active Directory
1. Open **Active Directory Users and Computers (ADUC)** on the Windows Server.
   ![Open_ADUC](images/43_Open_ADUC.png)  
2. (Optional) Create a new **OU** called `ServiceAccounts`.
   ![Created_ServiceAccounts_OU](images/1_Created_ServiceAccounts_OU.png)  
3. Right-click → **New → User**.
   ![New_User_ServiceAccounts](images/2_New_User_ServiceAccounts.png)
4. Configure the account:  
   - **Name:** `Website_FirstName LastName Login`  
   - **User logon name:** `$Website-Login`
     ![Website_Login_Create](images/3_Website_Login_Create.png)
   - **Password settings:**  
     - Uncheck **User must change password at next logon**  
     - Check **User cannot change password**  
     - Check **Password never expires**
   ![Password_Settings](images/4_Password_Settings.png)
   - Description: *“Auto-login to display websites”*  
5. Finish creating the account.
![Finish_Account](images/5_Finish_Account.png)
![Verify_Service_Account](images/6_Verify_Service_Account.png)
6. Description: *“Auto-login to display websites”*  
![Description_Website_Login](images/7_Description_Website_Login.png)
✅ Outcome: Service account created and ready for auto-login.

---

### 2️⃣ Install Sysinternals Suite on Windows Client
1. Download **Sysinternals Suite**.
![Sysinternals_Google](images/8_Sysinternals_Google.png)
![Download_Sysinternals_Suite](images/9_Download_Sysinternals_Suite.png)
2. Extract to a **network folder** accessible by IT staff.
![Select_Destination_File](images/10_Select_Destination_File.png)
3. Launch **Autologon64.exe** from the Sysinternals tools.
![Auto_Logon64](images/11_Auto_Logon64.png)
---

### 3️⃣ Configure Auto-Login for the Service Account
1. Open **Autologon64** → click **Yes** to agree.
   ![Agree_Sysinternal_Software](images/12_Agree_Sysinternal_Software.png)
  
2. Enter:
   - **Username:** `$Website-Login`  
   - **Domain:** `corp.local`  
   - **Password:** [service account password]  
3. Click **Enable** → reboot the Windows 10 client.  
![Autologon_Sysinternals_Username](images/13_Autologon_Sysinternals_Username.png)
![Success_Autologon](images/14_Success_Autologon.png)
✅ Outcome: Computer logs in automatically with the service account.
![Website_Login_Auto](images/15_Website_Login_Auto.png)
---

### 4️⃣ Set Browser to Auto-Start with Specific Web Page
1. Install **Google Chrome** (if not already installed).  
2. Open Chrome → **Settings → On startup → Open a specific page or set of pages** → Enter URL.
   ![Settings_Chrome](images/16_Settings_Chrome.png)
   ![On_Startup_Chrome](images/17_On_Startup_Chrome.png)
   ![Add_New_Page](images/18_Add_New_Page.png)
3. Website opens automatically when opening Chrome
   ![Startup_Website_Loads](images/19_Startup_Website_Loads.png)
4. Set Chrome shortcut to open in **full screen**:
   - Right-click Chrome shortcut → **Properties** → add `--start-Fullscreen` at the end of the **Target** field → Apply → OK.
   ![Properties_Chrome](images/20_Properties_Chrome.png)
![Shortcut_Start_Fullscreen](images/21_Shortcut_Start_Fullscreen.png)
5. ✅ Outcome: Browser opens automatically in full-screen mode showing the desired page.
![Verify_Fullscreen](images/22_Verify_Fullscreen.png)
6. Open **Run → `shell:startup`** and copy the Chrome shortcut there to launch automatically after login.  
![Shell_Startup](images/23_Shell_Startup.png)
![Google_Chrome_Startup](images/24_Google_Chrome_Startup.png)

---

### 5️⃣ Configure System Settings
1. Prevent the computer from sleeping:
   - Open **Settings → Power & Sleep** → Set **Sleep** to *Never*.
![Power_Sleep_Settings](images/25_Power_Sleep_Settings.png)
![Never_Sleep](images/26_Never_Sleep.png) 
2. Test by rebooting → Chrome should auto-launch in full screen.
![Verify_Sleep](images/27_Verify_Sleep.png)
---

### 6️⃣ Restrict Local Logon Using Group Policy
1. Open **GPMC** → navigate to domain → **Create a new GPO**: `Restrict Logon to Service Account`.
![GPMC_Service_Acc](images/28_GPMC_Service_Acc.png)
![Restrict_Logon_Service_Acc](images/29_Restrict_Logon_Service_Acc.png)
2. Right-click → **Edit** → go to:  Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → User Rights Assignment.
![Edit_GPO](images/30_Edit_GPO.png)
![Deny_Log_Locally_GPM](images/31_Deny_Log_Locally_GPM.png)

3. Before configuring **deny log on locally** go to ADUC, verify security group "AllEmployees" contains all standard users.
![All_Employees_SecGroup](images/32_All_Employees_SecGroup.png)
![Members_AllEmployees_Prop](images/33_Members_AllEmployees_Prop.png)
4. Configure **Deny log on locally**:
- **Add Group:** `AllEmployees` (contains all standard users)  
- Apply and OK.
![Select_AllEmployees_Object](images/34_Select_AllEmployees_Object.png)
![Add_AllEmployees](images/35_Add_AllEmployees.png)
![Apply_AllEmployees_Deny_Logon](images/36_Apply_AllEmployees_Deny_Logon.png)
![Verify_Deny_Logon_Locally](images/37_Verify_Deny_Logon_Locally.png)

5. Before linking the GPO, verify in ADUC that Workstations OU contains client computers.
   ![Work_Stations_ADUC](images/41_Work_Stations_ADUC.png) 
6. Link the GPO to the **OU containing your kiosk client** (Workstations).
![Work_Stations_GPO_Applied](images/38_Work_Stations_GPO_Applied.png)
7. Run `gpupdate /force` on the Windows 10 client.
![Gpupdate_Force](images/39_Gpupdate_Force.png)
✅ Outcome: Only the service account can log in locally; all other users are denied.
![Sign_Into_EveHr](images/40_Sign_Into_EveHr.png)
![Verify_Deny_EveHR_Login](images/42_Verify_Deny_EveHR_Login.png)

### 7️⃣ Verified the configurations applied below.
- Reboot the client machine → should log in automatically with the service account.  
- Chrome opens full-screen with the configured webpage.  
- Attempt to log in with a standard user → access should be denied.

---

✅ With this configuration, you have successfully created a **service account** for a kiosk-like computer, ensured auto-login, launched a full-screen webpage automatically, and restricted local logon to authorized accounts only.
