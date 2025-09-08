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
2. (Optional) Create a new **OU** called `ServiceAccounts`.  
3. Right-click → **New → User**.  
4. Configure the account:  
   - **Name:** `Website_FirstName LastName Login`  
   - **User logon name:** `$Website-Login`  
   - **Password settings:**  
     - Uncheck **User must change password at next logon**  
     - Check **User cannot change password**  
     - Check **Password never expires**  
   - Description: *“Auto-login to display websites”*  
5. Finish creating the account.

✅ Outcome: Service account created and ready for auto-login.

---

### 2️⃣ Install Sysinternals Suite on Windows Client
1. Download **Sysinternals Suite**.  
2. Extract to a **network folder** accessible by IT staff.  
3. Launch **Autologon64.exe** from the Sysinternals tools.

---

### 3️⃣ Configure Auto-Login for the Service Account
1. Open **Autologon64** → click **Yes** to agree.  
2. Enter:
   - **Username:** `$Website-Login`  
   - **Domain:** `corp.local`  
   - **Password:** [service account password]  
3. Click **Enable** → reboot the Windows 10 client.  

✅ Outcome: Computer logs in automatically with the service account.

---

### 4️⃣ Set Browser to Auto-Start with Specific Web Page
1. Install **Google Chrome** (if not already installed).  
2. Open Chrome → **Settings → On startup → Open a specific page or set of pages** → Enter URL.  
3. Set Chrome shortcut to open in **full screen**:
   - Right-click Chrome shortcut → **Properties** → add `-start-fullscreen` at the end of the **Target** field → Apply → OK.  
4. Open **Run → `shell:startup`** and copy the Chrome shortcut there to launch automatically after login.  

✅ Outcome: Browser opens automatically in full-screen mode showing the desired page.

---

### 5️⃣ Configure System Settings
1. Prevent the computer from sleeping:
   - Open **Settings → Power & Sleep** → Set **Sleep** to *Never*.  
2. Test by rebooting → Chrome should auto-launch in full screen.

---

### 6️⃣ Restrict Local Logon Using Group Policy
1. Open **GPMC** → navigate to domain → **Create a new GPO**: `Restrict Logon to Service Account`.  
2. Right-click → **Edit** → go to:  Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → User Rights Assignment.

3. Configure **Deny log on locally**:
- **Add Group:** `LabUsers` (contains all standard users)  
- Apply and OK.  
4. Link the GPO to the **OU containing your kiosk client**.  
5. Run `gpupdate /force` on the Windows 10 client.

✅ Outcome: Only the service account can log in locally; all other users are denied.



### 7️⃣ Verification
- Reboot the client machine → should log in automatically with the service account.  
- Chrome opens full-screen with the configured webpage.  
- Attempt to log in with a standard user → access should be denied.

---

✅ With this configuration, you have successfully created a **service account** for a kiosk-like computer, ensured auto-login, launched a full-screen webpage automatically, and restricted local logon to authorized accounts only.
