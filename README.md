# 🧰 Active Directory (AD) & User Account Management Lab (Windows Server + Windows 10/11 Client)

## 📖 Description
Learn how corporate IT environments manage users, groups, and authentication using Active Directory.

## 🖥️ Environments Used
| Role                      | OS                            | Purpose                              |
| ------------------------- | ----------------------------- | ------------------------------------ |
| 🏢 **Domain Controller**  | Windows Server 2022 (or 2019) | Host AD DS, DNS, and user management |
| 💻 **Client Workstation** | Windows 10 or 11              | Join to domain and test login        |
| 🧠 **Host OS**            | Windows 11                    | Runs both VMs via VirtualBox         |

 ---


## 🛠️ Lab Walkthrough

### 🔹 Step 1 – Create Your Virtual Network

Inside VirtualBox:

- Go to File → Host Network Manager → Create

- Name it something like ITLabNet

- Assign both VMs to this same internal network.
