# 🍜 Pho Anh Hai – Windows Sysadmin Infrastructure Lab  
**Inspired by the Vietnamese indie horror game _Brother Hai’s Pho Restaurant_**  
This project builds a full Windows Server enterprise environment for a fictional restaurant company: **Pho Anh Hai Co. Ltd**.  
The lab uses VMware Workstation, Windows Server 2025, and Windows 11.

The goal:  
✔ Learn real System Administration  
✔ Practice Active Directory, DNS, DHCP, GPO, DFS, PKI  
✔ Build a professional portfolio project  
✔ Simulate the IT system of a small restaurant chain

✨ All configuration steps, screenshots, and topology diagrams are included below.

# Network Topology
![Topology](screenshots/35_Topology_Diagram.png)


---

# 1. 📡 Network Setup

### VMware Network Configuration  
All VMs use **VMnet10 – Host-only**  
- Subnet: `10.10.10.0/24`  
- DHCP from VMware: **disabled**  
- DHCP managed by SERVER-DC  

**Screenshot:**  
![vmnet10](screenshots/01_VMnet10_Config.png)

---

# 2. 🏢 Server Overview

| Server Name | Function | IP |
|-------------|----------|----|
| **SERVER-DC** | Domain Controller, DNS, DHCP, PKI | 10.10.10.10 |
| **SERVER-FILE** | File Server + DFS Namespace | 10.10.10.20 |
| **SERVER-MGMT** | Admin Tools (RSAT, GPO, ADUC) | 10.10.10.30 |

**Screenshot – All servers running:**  
![ServersRunning](screenshots/02_AllServersRunning.png)

---

# 3. 🧱 SERVER-DC Setup  
Domain Controller + DNS + DHCP + PKI

## 3.1 Rename PC → SERVER-DC  
![DC_Rename](screenshots/03_SERVER-DC_Rename.png)

## 3.2 Static IP Configuration  
IP: `10.10.10.10`  
DNS: `10.10.10.10`  
![DC_IP](screenshots/04_SERVER-DC_IP.png)

## 3.3 Install AD DS  
Server Manager → Add Roles → **Active Directory Domain Services**  
![ADDS_Role](screenshots/05_ADDS_Role.png)

## 3.4 Promote to Domain Controller  
Domain: `phoanhhai.local`  
![PromoteDC](screenshots/06_Promote_DC.png)

## 3.5 Install DHCP  
![DHCP_Role](screenshots/07_DHCP_Role.png)

## 3.6 Create DHCP Scope  
Range: `10.10.10.100–10.10.10.200`  
DNS Option: `10.10.10.10`  
![DHCP_Scope](screenshots/08_DHCP_Scope.png)

## 3.7 Install PKI (Certification Authority)  
Enterprise Root CA  
![PKI_CA](screenshots/09_PKI_CA.png)

---

# 4. 🗂️ Active Directory Structure  
OU: **Pho Anh Hai Co. Ltd**  
Sub-OUs: Kitchen, Cashier, Service Staff, Delivery, Management, Servers, Groups  

**Screenshot:**  
![AD_OU](screenshots/10_AD_OU_Structure.png)

## 4.1 Security Groups  
- GG_Kitchen  
- GG_Cashier  
- GG_ServiceStaff  
- GG_Delivery  
- GG_Management  

![AD_Groups](screenshots/11_AD_Groups.png)

## 4.2 Department Users  
Example:  
- kitchen.user  
- cashier.user  
- service.user  
- delivery.user  
- manager.user  

![AD_Users](screenshots/12_AD_Users.png)

---

# 5. 📁 SERVER-FILE Setup (DFS + NTFS)

## 5.1 Rename to SERVER-FILE & join domain  
![ServerFile_Join](screenshots/13_SERVER-FILE_JoinDomain.png)

## 5.2 Create Shared Folders  
```
E:\Shares\Recipes
E:\Shares\Invoices
E:\Shares\HRDocs
```

![File_Structure](screenshots/14_File_Structure.png)

## 5.3 Share & NTFS Permissions  
### Recipes  
- GG_Kitchen → Modify  
- GG_Management → Modify  
- GG_ServiceStaff → Read  

![Recipes_Permissions](screenshots/15_Recipes_NTFS.png)

### Invoices  
- GG_Cashier → Modify  
- GG_Management → Modify  

![Invoices_Permissions](screenshots/16_Invoices_NTFS.png)

### HRDocs  
- GG_Management → Full Control  

![HRDocs_Permissions](screenshots/17_HRDocs_NTFS.png)

## 5.4 DFS Namespace  
Namespace path:

``` \\phoanhhai.local\CompanyData ```

**Screenshot:**  
![DFS_Namespace](screenshots/18_DFS_Namespace.png)

---

# 6. 🛠️ SERVER-MGMT Setup (Admin Tools)

## 6.1 Rename + Join Domain  
![MGMT_Join](screenshots/19_SERVER-MGMT_JoinDomain.png)

## 6.2 Install Admin Tools (RSAT)  
Installed tools:
- Active Directory Users and Computers  
- Group Policy Management  
- DNS Manager  
- DHCP Manager  
- Certificate Authority Tools  
- DFS Management Tool  

**Screenshot:**  
![RSAT_Installed](screenshots/20_RSAT_Installed.png)

---

# 7. 🧑‍💻 Client Setup  

All Windows 11 clients use **DHCP**.

**Clients:**
- CLIENT-KITCHEN  
- CLIENT-CASHIER  
- CLIENT-SERVICE  
- CLIENT-DELIVERY  

## 7.1 DHCP Lease Confirmation  
![DHCP_Leases](screenshots/21_DHCP_Leases.png)

## 7.2 Join Domain & Move to Correct OU  
![Client_Join](screenshots/22_Client_JoinDomain.png)

---

# 8. 🔐 Group Policy (GPO)

## 8.1 Password Policy (Domain Level)  
![PasswordPolicy](screenshots/23_GPO_PasswordPolicy.png)

## 8.2 Desktop Wallpaper (Branding)  
![Wallpaper_GPO](screenshots/24_GPO_Wallpaper.png)

## 8.3 USB Block for Cashier Department  
Applied only to **OU: Cashier**

Policies:
- Removable Disks: Deny read  
- Removable Disks: Deny write  
- Removable Disks: Deny execute  

**Screenshot:**  
![USB_Block](screenshots/25_GPO_USB_Block.png)

---

# 9. 🔑 PKI – Certificate Auto-Enrollment

## 9.1 Auto-Enrollment GPO  
![AutoEnroll_GPO](screenshots/26_Cert_AutoEnroll_GPO.png)

## 9.2 Client Certificate Received  
![Client_Cert](screenshots/27_Client_Cert_Installed.png)

---

# 10. 🧪 Client Testing (Real Business Simulation)

## 10.1 CLIENT-KITCHEN  
- Can modify **Recipes**  
- Cannot access **Invoices**  

![Kitchen_EditRecipes](screenshots/28_Kitchen_EditRecipes.png)  
![Kitchen_DeniedInvoices](screenshots/29_Kitchen_DeniedInvoices.png)

---

## 10.2 CLIENT-CASHIER  
- USB fully blocked  
- Can modify **Invoices**  

![Cashier_USB_Blocked](screenshots/30_Cashier_USB_Blocked.png)  
![Cashier_EditInvoices](screenshots/31_Cashier_EditInvoices.png)

---

## 10.3 CLIENT-SERVICE  
- Recipes = Read-only  
- Wallpaper applied  

![Service_ReadOnly](screenshots/32_Service_ReadRecipes.png)  
![Service_Wallpaper](screenshots/33_Service_Wallpaper.png)

---

## 10.4 CLIENT-DELIVERY  
- Access **Recipes** (read only)  

![Delivery_ReadRecipes](screenshots/34_Delivery_ReadRecipes.png)

---

# 11. 🎯 What This Project Demonstrates

This lab shows full Windows Sysadmin capabilities:

- Active Directory and OU design  
- DNS & DHCP administration  
- Group Policy enforcement  
- File Server with NTFS permissions  
- DFS namespace  
- Internal PKI & certificate auto-enrollment  
- Client configuration & department-based access control  
- USB security policy  
- Realistic enterprise documentation workflow  

---

# 12. ❤️ Inspiration  
This project is inspired by the Vietnamese indie horror game:
**_Brother Hai’s Pho Restaurant_**  
but implemented in a **fully professional IT infrastructure design**.

---

# 13. 📬 Contact  
Created by: **Tommy Huynh**  
Use this project for learning, portfolio work, and system administration practice.

