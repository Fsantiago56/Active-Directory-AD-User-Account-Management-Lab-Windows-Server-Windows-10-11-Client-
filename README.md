# 🧩 Active Directory (AD) & User Account Management Lab (Windows Server + Windows 10/11 Client)

## 📖 Description
This lab demonstrates how to deploy and manage **Active Directory Domain Services (AD DS)** on **Windows Server 2022** and connect a **Windows 10/11 client** to the domain.  
The lab simulates a real-world IT environment for learning **centralized authentication**, **user management**, and **domain-based security**.

---

## 🧠 Learning Objectives
By the end of this lab, you will be able to:
- Configure a Windows Server as a **Domain Controller**
- Create and manage **Organizational Units (OUs)** and **user accounts**
- Join a **Windows client** to a domain
- Log in and manage accounts through **Active Directory**

---

## 🖥️ Environments Used

| Component | Configuration |
|------------|---------------|
| **Host OS** | Windows 11 |
| **Virtualization** | VirtualBox |
| **Server VM** | Windows Server 2022 Standard Evaluation |
| **Client VM** | Windows 10 / Windows 11 |
| **Network Type** | VirtualBox Host-Only Ethernet Adapter |
| **Domain Name** | `company.local` |

---

## 🧩 Network Overview

### 🌐 Network Mode: VirtualBox Host-Only Ethernet Adapter
VirtualBox automatically creates a **Host-Only network** between your **host PC**, **Windows Server VM**, and **Windows Client VM**.  
This allows them to communicate **locally** without using your home Wi-Fi or router.

## 🛠️ Lab Walkthrough

### 🔹 Step 1 – Create Virtual Machines
**Description:**  
Set up two VirtualBox VMs to simulate a small network domain environment.

**Goal:**  
Prepare the foundation for a server–client architecture.

| VM | OS | RAM | CPU | Network |
|----|----|-----|-----|-------|
| `DC01` | Windows Server 2022 | 4 GB | 2 | VirtualBox Host-Only Ethernet Adapter |
| `Client01` | Windows 10/11 | 4 GB | 2 | VirtualBox Host-Only Ethernet Adapter |

</details> <details> <summary>📸 Click to view Virtual Machine ScreenShots</summary>
<p align="center">
  ✅ <strong>Creation Of Domain Controller VM</strong> ✅  
  <br>
  <img src="https://i.imgur.com/mv2SFrD.png" width="60%">
  </p>
 <p align="center">
  ✅ <strong>Creation Of Windows 11 VM</strong> ✅  
  <br>
  <img src="https://i.imgur.com/cNFOqGC.png" width="70%">
  </p>
</details>

---

### 🔹 Step 2 – Configure Static IP and Rename Server
**Description:**  
Give the server a permanent IP address and meaningful hostname.

**Goal:**  
Ensure reliable communication between server and client in the AD domain.

<details>
<summary>📌 Show Commands</summary>

```powershell
Rename-Computer -NewName "DC01" -Restart
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.10.10 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1
```
</details>

</details> <details> <summary>📸 Click to view  ScreenShots</summary>
<p align="center">
  ✅ <strong>Verify Settings</strong> ✅  
  <br>
  <img src="https://i.imgur.com/lo5xAKv.png" width="60%">
  </p>

</details>

### 🔹 Step 3 – Install Active Directory Domain Services (AD DS)
**Description:**  
This step installs the Active Directory Domain Services (AD DS) role on your Windows Server 2022 VM.
AD DS is what allows your server to manage users, computers, and security policies in a centralized domain.

## 🎯 Goal:

Prepare your Windows Server to act as a Domain Controller (DC).

🪜 Steps:

-  On DC01, log in as Administrator.

-  Open Server Manager (it opens automatically at login, or search for it).

-  Click Manage → Add Roles and Features.

-  Click Next several times until you reach Server Roles.

-  Check the box for Active Directory Domain Services.

-  lick Add Features → Next → Install.

- Wait for the installation to finish — don’t restart yet.

</details> <details> <summary>📸 Click to view  ScreenShot</summary>
<p align="center">
  ✅ <strong>Verify Installation</strong> ✅  
  <br>
  <img src="https://i.imgur.com/2KL8OcH.png" width="60%">
  </p>

</details>
