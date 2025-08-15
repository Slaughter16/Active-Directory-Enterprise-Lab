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
   - Save Configuration

  ![Disable_DHCP_pfSense](images/8_Disable_DHCP.png)
  ![Disable_DHCP_pfSense](images/9_Disable_DHCP.png)
  ![Disable_DHCP_pfSense](images/10_Disable_DHCP.png)

  
3. **Installed DHCP Role on Windows Server 2019**  
   - Server Manager → Add Roles and Features → DHCP Server  
   - Completed post-install configuration and authorized in AD  

  ![Install_DHCP_WINServer](images/11_Install_WINServer.png)
  ![Install_DHCP_WINServer](images/12_Install_WINServer.png)
  ![Install_DHCP_WINServer](images/13_Install_WINServer.png)
  ![Install_DHCP_WINServer](images/14_Install_WINServer.png)
  ![Install_DHCP_WINServer](images/15_Install_WINServer.png)
  ![Install_DHCP_WINServer](images/16_Install_WINServer.png)
  ![Install_DHCP_WINServer](images/17_Install_WINServer.png)
  ![Install_DHCP_WINServer](images/18_Install_WINServer.png)

---

4. **Created New DHCP Scope**  
   - Scope Name: VLAN10-Scope  
   - IP Range: 192.168.10.50–192.168.10.200  
   - Exclusions: 192.168.10.1–192.168.10.49  
   - Default Gateway: 192.168.10.1  
   - DNS Servers: 192.168.10.10, 192.168.20.2  
   - Activated scope  

  ![Create_DHCP_Scope](images/19_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/20_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/21_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/22_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/23_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/24_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/25_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/26_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/27_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/28_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/29_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/30_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/31_DHCP_Scope.png)





---

5. **Tested DHCP on Clients**

**Windows 10 Client:**  
```cmd
ipconfig /release       # Release current IP
ipconfig /renew         # Request new IP from DHCP
ipconfig /all           # Verify IP, gateway, DHCP server, and DNS
```

  ![WIN_10_Verify](images/32_WIN10_Verify.png)
  ![WIN_10_Verify](images/33_WIN10_Verify.png)
  ![WIN_10_Verify](images/34_WIN10_Verify.png)






  **Windows 11 Client**:  
 ```cmd
   ipconfig /release
   ipconfig /renew
   ipconfig /all
```
  ![WIN_11_Verify](images/35_WIN11_Verify.png)
  ![WIN_11_Verify](images/36_WIN11_Verify.png)


  **Debian Client**:
 ```cmd
  sudo dhclient -r      # Release current IP
  sudo dhclient         # Request new IP from DHCP
  ip a                  # Check assigned IP address
  ip route              # Verify default gateway
  cat /etc/resolv.conf  # Check DNS servers
```
