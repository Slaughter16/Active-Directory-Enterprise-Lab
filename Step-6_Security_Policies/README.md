# Step 6 – Implementing Security Policies

## Password Policy Configuration

To enforce strong password standards across the domain, I configured the **Default Domain Policy** in Group Policy Management.  

**Path in GPO Editor:**  
`Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy`

### Configured Settings:
- **Enforce Password History:** 4 passwords remembered  
- **Maximum Password Age:** 60 days  
- **Minimum Password Age:** 1 day  
- **Minimum Password Length:** 7 characters  
- **Password Must Meet Complexity Requirements:** Enabled  

### Why These Settings?
- **Password History** prevents users from recycling recent passwords.  
- **Maximum Age** forces regular password changes.  
- **Minimum Age** stops users from cycling through multiple changes in one sitting.  
- **Minimum Length + Complexity** enforces stronger, harder-to-crack passwords.  

### Testing
- On the **IT client** machine, logged in as a domain user.  
- Attempted to set a weak password (e.g., `password1`) → **Rejected**.  
- Set a strong password (e.g., `P@ssw0rd!2025`) → **Accepted**.  
- Verified policy enforcement via **RSOP.msc** and **gpresult /r**.  
