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

### 🔹 Step 4 – Promote Server to Domain Controller
**Description:**  
Now that AD DS is installed, you’ll promote your server into a Domain Controller (DC) and create a new domain — the backbone of your Active Directory network.

## 🎯 Goal:
Create and configure a domain named company.local.

🪜 Steps:

-  In Server Manager, click the ⚠️ yellow triangle at the top.
→ Select “Promote this server to a domain controller.”

-  Choose Add a new forest (since this is your first DC).

-  In the Root domain name, type: company.local

-  Click Next and create a DSRM password (used for recovery only).

-  Keep default settings for DNS and NetBIOS name.

- Continue clicking Next → Install.

- The server will automatically restart when finished.

 <details> <summary>📸 Click to view  ScreenShot</summary>
<p align="center">
  ✅ <strong>Verify Domain configuration</strong> ✅  
  <br>
  <img src="https://i.imgur.com/yLpslxV.png" width="60%">
  </p>
   
</details>

### 🔹 Step 5 – Create Organizational Units (OUs) and Users
**Description:**  
Organizational Units (OUs) are folders inside Active Directory used to organize users and computers.
You’ll create a few OUs for different departments and add a test user.

🎯 Goal:

Simulate a company structure and prepare domain accounts for testing.

🪜 Steps :

Open Server Manager → Tools → Active Directory Users and Computers (ADUC).

Right-click your domain → New → Organizational Unit.

Create these OUs:

- IT

- HR

- Sales

Inside each OU, right-click → New → User → create:

```
- Different Logon for Each Organizational Unit

john.it@company.local
john.hr@company.local
john.sales@company.local

- Password for Each User

Password: P@ssw0rd!
```


<details> <summary>📸 Click to view  ScreenShot</summary>
<p align="center">
  ✅ <strong>Verify Creation of Organizational Units & Users</strong> ✅  
  <br>
  <img src="https://i.imgur.com/PYcc217.png" width="60%">
  </p>
   
</details>

### 🔹 Step 6 – Configure Windows Client (Client01)
**Description:**  
Before joining your client VM to the domain, you need to give it a static IP address and make sure its DNS points to your Domain Controller.

🎯 Goal:

Make Client01 communicate directly with DC01 on the same network.

🪜 Steps:

- Log in to Client01 (your Windows 10/11 VM).

- Open PowerShell (as Administrator).

- Type these commands:

```powershell
Rename-Computer -NewName "Client01" -Restart
```
→ After reboot, reopen PowerShell and enter:

```powershell
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.56.20 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.56.10
```


<details> <summary>📸 Click to view  ScreenShot</summary>
<p align="center">
  ✅ <strong>Verify Client network properties </strong> ✅  
  <br>
  <img src="https://i.imgur.com/CRQj6kE.png" width="60%">
  </p>
   
</details>

### 🔹 Step 7 – Join the Client to the Domain
**Description:**  
Now you’ll join the client computer to the company.local domain so it can log in using domain credentials.

🎯 Goal:

Enable domain-based logins and centralized policy control.

🪜 Steps:

1. On Client01, Right-click Start → System (or press Windows + Pause/Break)
2. Click Advanced system settings on the right
3. Under the Computer Name tab → click Change...
4. In the “Member of” section → select Domain
5. Enter:
   ```
   company.local
   ```
6. When prompted, enter domain credentials:
   ```
   Administrator@company.local
   ```
7. After success, you’ll get a “Welcome to the company.local domain” message
8. Restart the computer when prompted.


<details> <summary>📸 Click to view  ScreenShot</summary>
<p align="center">
  ✅ <strong>Verify Domain join success confirmation screen. </strong> ✅  
<br>
<img src="https://i.imgur.com/hbthTZv.png" width="60%">
</p>
    
</details>
     
### 🔹 Step 8 – Log In as a Domain User
**Description:**  
You’ll now verify that your client can log in using the domain user account you created earlier.

🎯 Goal:

Confirm the client successfully authenticates to the domain controller.

🪜 Steps:

On Client01, at the login screen, click Other User.

Enter:
```
Username: john.it@company.local
Password: P@ssw0rd!
```

Press Enter and wait — the login may take a few moments while Windows creates the new profile.

<details> <summary>📸 Click to view  ScreenShot</summary>
<p align="center">
  ✅ <strong>Verify Successful login as john.it on the domain. </strong> ✅  
<br>
<img src="https://i.imgur.com/pWcfLZx.png" width="60%">
</p>
  
</details>


