# Step 2 – Install Active Directory Domain Services & Join Clients

## 📌 Objective
Install Active Directory Domain Services (AD DS) on Windows Server 2019/2022, configure the domain, and join client machines to the domain.  
This creates the foundation for user management, Group Policy application, and enterprise network administration.

---

## 🔹 Background
- Active Directory provides centralized authentication, authorization, and management for users, computers, and resources.  
- Joining clients to a domain allows administrators to enforce policies, manage users, and deploy resources efficiently.  
- OU (Organizational Unit) structures help segment users and computers logically for easier administration.

---

## 🛠️ Configuration Details
- **Domain Name:** lab.local  
- **Domain Controller:** Windows Server 2019/2022 (DC01)  
- **Static IP of DC01:** 192.168.10.10  
- **DNS Server:** 192.168.10.10  
- **Clients to Join:** Windows 10, Windows 11, Debian

- **OU Structure Example:**

