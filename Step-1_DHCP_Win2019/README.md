# Step 1 – Migrate DHCP from pfSense to Windows Server 2019

## 📌 Objective
Migrate DHCP services from pfSense to Windows Server 2019 for VLAN 10.  
This centralizes network services in an AD environment, allowing integrated DNS updates, better logging, and enterprise-like management.

---

## 🔹 Background
- In real-world Active Directory environments, DHCP is typically managed by Windows Server rather than a firewall/router.  
- Centralized DHCP simplifies network management, integrates with DNS, and improves client provisioning.

---

## 🛠️ Configuration Details
- **DHCP Server:** Windows Server 2019  
- **Scope Name:** VLAN10-Scope  
- **IP Range:** 192.168.10.50 – 192.168.10.200  
- **Exclusions:** 192.168.10.1 – 192.168.10.49  
- **Subnet Mask:** 255.255.255.0  
- **Default Gateway:** 192.168.10.1  
- **DNS Servers:**  
  - Primary: 192.168.10.10 (Windows Server DNS)  
  - Secondary: 192.168.20.2  

---

## 🔹 Steps Performed

### 1. **Assigned Static IP to Windows Server 2019**
  - Opened `Network & Internet Settings → Change Adapter Options → Ethernet → Properties`.
   ![Assign_Static_IP_WIN11](images/1_Static_WIN11.png)
   ![Assign_Static_IP_WIN11](images/2_Static_WIN11.png)
   ![Assign_Static_IP_WIN11](images/3_Static_WIN11.png)
   ![Assign_Static_IP_WIN11](images/4_Static_WIN11.png)

   - Selected `Internet Protocol Version 4 (TCP/IPv4)` → `Use the following IP address`.
  
   ![Assign_Static_IP_WIN11](images/5_Static_WIN11.png)

   - Configured the following:

```bash
IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
Preferred DNS: 192.168.10.10
Alternate DNS: 192.168.20.2
```
    
  ![Assign_Static_IP_WIN11](images/6_Static_WIN11.png)

   - Confirmed settings via

  ```cmd
  `ipconfig /all`.
  ```
  ![Assign_Static_IP_WIN11](images/7_Static_WIN11.png)

---

2. **Disabled DHCP on pfSense VLAN 10**  
   - Navigated to `Services → DHCP Server`  
   - Disabled DHCP for VLAN 10 only  
   - Verified service stopped  

  ![Disable_DHCP_pfSense](8_Disable_DHCP.png)
  ![Disable_DHCP_pfSense](9_Disable_DHCP.png)
  ![Disable_DHCP_pfSense](10_Disable_DHCP.png)

  
3. **Installed DHCP Role on Windows Server 2019**  
   - Server Manager → Add Roles and Features → DHCP Server  
   - Completed post-install configuration and authorized in AD  

   ![DHCP Role Installation](images/dhcp_scope_config.png)

---

4. **Created New DHCP Scope**  
   - Scope Name: VLAN10-Scope  
   - IP Range: 192.168.10.50–192.168.10.200  
   - Exclusions: 192.168.10.1–192.168.10.49  
   - Default Gateway: 192.168.10.1  
   - DNS Servers: 192.168.10.10, 192.168.20.2  
   - Activated scope  

   ![DHCP Scope Configuration](images/dhcp_scope_config.png)

---

5. **Tested DHCP on Clients**

   **Windows 10 Client**:  
   ```cmd
   ipconfig /release
   ipconfig /renew
   ipconfig /all
