# 🍜 Pho Anh Hai Sysadmin Lab  
**Inspired by the Vietnamese horror game _Brother Hai’s Pho Restaurant_**

This project is a full Windows Server System Administration lab designed to simulate the IT infrastructure of a small business: **Pho Anh Hai Co. Ltd**.  
The goal is to build a realistic, enterprise-style environment using VMware Workstation, Windows Server 2025, and Windows 11.

The lab includes:
- Active Directory Domain Services  
- DNS, DHCP  
- Group Policy (GPO)  
- DFS File Server  
- Internal PKI (Certificate Authority)  
- Department-based clients with different permissions  

Everything is set up in a clean and professional way, following real-world IT system administration practices.

---

## 1. Lab Overview

**Domain name:** `phoanhhai.local`  
**Inspiration:** Brother Hai’s Pho Restaurant (Vietnamese horror story-driven game)  
**Topology:** 3 servers + 4 clients  

### Servers (Windows Server 2025)
- `SERVER-DC` — Domain Controller, DNS, DHCP, PKI  
- `SERVER-FILE` — File Server + DFS namespace  
- `SERVER-MGMT` — Management Server (RSAT, GPO, AD tools)  

### Clients (Windows 11)
- `CLIENT-KITCHEN` — Kitchen staff  
- `CLIENT-CASHIER` — Cashier / POS (USB locked)  
- `CLIENT-SERVICE` — Service staff  
- `CLIENT-DELIVERY` — Delivery staff  

The environment simulates how a real restaurant company could use IT systems for internal operations.

---

## 2. Network Topology

### Network
- VMware Host-Only Network: **VMnet10**  
- Subnet: `10.10.10.0/24`  
- DHCP: provided by SERVER-DC  

### IP Plan
| Device | IP | Notes |
|--------|---------|-------|
| SERVER-DC | 10.10.10.10 | AD DS / DNS / DHCP / PKI |
| SERVER-FILE | 10.10.10.20 | File server |
| SERVER-MGMT | 10.10.10.30 | Management tools |
| Clients | DHCP | 10.10.10.100–200 |

---

## 3. Active Directory Structure
```
phoanhhai.local
└── 🏢 Pho Anh Hai Co. Ltd
    ├── 🖥️ Servers
    ├── 🍳 Kitchen
    ├── 🧾 Cashier
    ├── 👨‍🍳 Service Staff
    ├── 🚚 Delivery
    ├── 👔 Management
    └── 👥 Groups
```



### Security Groups
- GG_Kitchen  
- GG_Cashier  
- GG_ServiceStaff  
- GG_Delivery  
- GG_Management  

---

## 4. File Server & DFS

**DFS Namespace:**  
`\\phoanhhai.local\CompanyData`

**Shared folders:**
- Recipes  
- Invoices  
- HRDocs  

### Permission Model  
| Folder | Kitchen | Cashier | Service | Delivery | Management |
|--------|---------|---------|---------|----------|-------------|
| Recipes | Modify | No | Read | Read | Modify |
| Invoices | No | Modify | No | No | Modify |
| HRDocs | No | No | No | No | Full |

This simulates real-world access control inside a restaurant company.

---

## 5. Group Policy (GPO)

### Domain-wide
- Password policy  
- Account lockout policy  
- Wallpaper branding (company wallpaper)

### Department GPO
- **Cashier workstation USB locked**  
  - Block read / write / execute for removable storage  
- Auto-enroll certificates from PKI  
- Desktop configuration for consistency  

---

##  6. Client Roles

### `CLIENT-KITCHEN`
- Modify Recipes  
- Read-only for other folders  
- Standard workstation

### `CLIENT-CASHIER`
- Modify Invoices  
- **USB completely blocked** (security)  
- POS-style restricted workstation

### `CLIENT-SERVICE`
- Read Recipes  
- Basic staff workstation

### `CLIENT-DELIVERY`
- Read Recipes  
- Used for delivery schedules and printing

---

## 7. Project Goal

This project shows:
- How to design a small-business Windows Server environment  
- How to use AD, DNS, DHCP, GPO, DFS, and PKI  
- How to manage users, groups, departments, and permissions  
- How to lock down sensitive workstations (Cashier)  
- How IT supports real-world restaurant operations  

Although inspired by the “Brother Hai’s Pho Restaurant” game, this project is **100% professional and realistic**, following enterprise system administration standards.

---

## 8. How To Use This Lab

You can:
- Rebuild the whole lab for learning Windows Sysadmin  
- Practice AD/GPO administration  
- Demonstrate your IT skills in your portfolio  
- Use it for job interviews  
- Expand it with more services (WSUS, App Server, SIEM, etc.)

---

## ❤️ Credits

- **Inspired by:** Brother Hai’s Pho Restaurant (Indie Vietnamese horror game)  
- **Created by:** Tommy Huynh  
- **Purpose:** System Administration Portfolio Project (2025)  

---

## 📬 Contact
Feel free to reach out if you want to learn or collaborate.  


