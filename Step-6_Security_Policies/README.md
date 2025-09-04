# Step 6 – Implementing Security Policies

To enforce strong password standards across the domain, I configured the **Default Domain Policy** in Group Policy Management

---

## Password Policy Configuration

### Steps
1. Open **Group Policy Management Console (GPMC)**.  
   ![Screenshot – Open GPMC](screenshot1.png)

2. Navigate to **Group Policy Objects**, right-click **Default Domain Policy**, and select **Edit**.  
   ![Screenshot – Edit Default Domain Policy](screenshot2.png)

3. In the Group Policy Management Editor, go to:  
   `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy`  
   ![Screenshot – Password Policy Settings](screenshot3.png)

---

### Configured Settings:
- **Enforce Password History:** 24 passwords remembered  
- **Maximum Password Age:** 60 days  
- **Minimum Password Age:** 1 day  
- **Minimum Password Length:** 12 characters  
- **Password Must Meet Complexity Requirements:** Enabled  

screenshot 
### Why These Settings?
- **Password History** prevents users from recycling recent passwords.  
- **Maximum Age** forces regular password changes.  
- **Minimum Age** stops users from cycling through multiple changes in one sitting.  
- **Minimum Length + Complexity** enforces stronger, harder-to-crack passwords.  

---

### Testing
- On the **IT client** machine, logged in as a domain user.  
- Attempted to set a weak password (e.g., `password1`) → **Rejected**.  
- Set a strong password (e.g., `P@ssw0rd!2025`) → **Accepted**.  
- Verified policy enforcement via **RSOP.msc** and **gpresult /r**.  
