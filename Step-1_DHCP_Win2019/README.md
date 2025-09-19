# Step 1 – Migrate DHCP from pfSense to Windows Server 2019

## 📑 Table of Contents
- [📌 Objective](#-objective)
- [🔹 Background](#-background)
- [🛠️ Configuration Overview](#️-configuration-overview)
- [🔹 Steps Performed](#-steps-performed)
  - [1. Assigned Static IP to Windows Server 2019](#1-assigned-static-ip-to-windows-server-2019)
  - [2. Disabled DHCP on pfSense VLAN 10](#2-disabled-dhcp-on-pfsense-vlan-10)
  - [3. Installed DHCP Role on Windows Server 2019](#3-installed-dhcp-role-on-windows-server-2019)
  - [4. Created New DHCP Scope](#4-created-new-dhcp-scope)
  - [5. Tested DHCP on Clients](#5-tested-dhcp-on-clients)
    - [Windows 10 Client](#windows-10-client)
    - [Windows 11 Client](#windows-11-client)
    - [Debian Client](#debian-client)
  













## 📌 Objective
Migrate DHCP services from pfSense to Windows Server 2019 for VLAN 10.  
This centralizes network services in an AD environment, enabling:
- Intergrated DNS updates
- Better logging
- Enterprise-like management

---

## 🔹 Background
- In real-world Active Directory environments, DHCP is usually managed by **Windows Server**, not firewalls/routers.  
- Benefits of centralized DHCP:
    - Simplified network management
    - Automatic DNS integration
    - Improved reliability and control

---

## 🛠️ Configuration Overview
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

### 2. **Disabled DHCP on pfSense VLAN 10**  
   - Navigated to `Services → DHCP Server`  
   - Disabled DHCP for VLAN 10 only  
   - Save Configuration

  ![Disable_DHCP_pfSense](images/8_Disable_DHCP.png)
  ![Disable_DHCP_pfSense](images/9_Disable_DHCP.png)
  ![Disable_DHCP_pfSense](images/10_Disable_DHCP.png)

  
### 3. **Installed DHCP Role on Windows Server 2019**  
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

### 4. **Created New DHCP Scope**  
   - Scope Name: VLAN10-Scope  
   - IP Range: 192.168.10.50–192.168.10.200  
   - Exclusions: 192.168.10.1–192.168.10.49  
   - Default Gateway: 192.168.10.1  
   - DNS Servers: 192.168.10.10, 192.168.20.2  
   - Activated scope  

- Tools > DHCP
  ![Create_DHCP_Scope](images/19_DHCP_Scope.png)
- Win-server > IPv4 > More Actions
  ![Create_DHCP_Scope](images/20_DHCP_Scope.png)
- Click Next in the Wizard scope setup 
  ![Create_DHCP_Scope](images/21_DHCP_Scope.png)
  
  - Scope Name: VLAN10-Scope
  - Description: VLAN10-Leases
  ![Create_DHCP_Scope](images/22_DHCP_Scope.png)
- IP Range: 192.168.10.1 – 192.168.10.200
- Length: 24 , Subnet mask: 255.255.255.0
  ![Create_DHCP_Scope](images/23_DHCP_Scope.png)
- Exclusions: 192.168.10.1 – 192.168.10.49
  ![Create_DHCP_Scope](images/24_DHCP_Scope.png)
- Lease Duration: 8 Days
  ![Create_DHCP_Scope](images/25_DHCP_Scope.png)
- Yes, Configure DHCP Options
  ![Create_DHCP_Scope](images/26_DHCP_Scope.png)
- Default Gateway: 192.168.10.1
  ![Create_DHCP_Scope](images/27_DHCP_Scope.png)
- DNS Servers: 192.168.10.10, 192.168.20.2
  ![Create_DHCP_Scope](images/28_DHCP_Scope.png)
- Click Next 
  ![Create_DHCP_Scope](images/29_DHCP_Scope.png)
- Activated scope & Finish
  ![Create_DHCP_Scope](images/30_DHCP_Scope.png)
  ![Create_DHCP_Scope](images/31_DHCP_Scope.png)





---

### 5. **Tested DHCP on Clients**

### **Windows 10 Client:**  
```cmd
ipconfig /release       # Release current IP
ipconfig /renew         # Request new IP from DHCP
ipconfig /all           # Verify IP, gateway, DHCP server, and DNS
```

  ![WIN_10_Verify](images/32_WIN10_Verify.png)
  ![WIN_10_Verify](images/33_WIN10_Verify.png)
  ![WIN_10_Verify](images/34_WIN10_Verify.png)






### **Windows 11 Client**:  
 ```cmd
   ipconfig /release
   ipconfig /renew
   ipconfig /all
```
  ![WIN_11_Verify](images/35_WIN11_Verify.png)
  ![WIN_11_Verify](images/36_WIN11_Verify.png)


### **Debian Client**:
 ```cmd
  sudo dhclient -r      # Release current IP
  sudo dhclient         # Request new IP from DHCP
  ip a                  # Check assigned IP address
  ip route              # Verify default gateway
  cat /etc/resolv.conf  # Check DNS servers
```
  ![Debian_Verify](images/38_Debian_Verify.png)

