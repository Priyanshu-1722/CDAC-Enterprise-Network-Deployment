# CDAC Enterprise Network Deployment

This project showcases a complete enterprise-level network deployment using **VirtualBox** with a focus on **Windows Server 2022**, **Active Directory**, **DHCP**, **WDS**, **FSRM**, **IIS**, **FTP**, **WSB**, **Data Deduplication**, **Exchange Server**, **VPN**, **NLB**, and **Virtual Hosting**.

All virtual machines are configured within an **Internal Network (192.168.100.0/25)**. This environment simulates a real-world corporate IT infrastructure and serves as a hands-on learning lab for Windows Server administration, Active Directory, domain configuration, and enterprise services management.

---

## 🖥️ Virtual Machines Configuration

### 1. PARAM - Main Server (Windows Server 2022)
- **Hostname**: `PARAM`
- **IP**: `192.168.100.100`
- **Domain Controller**: `cdac.local`
- **RAM**: 6 GB
- **Disks**:
  - Disk 1: 50 GB (Partitions: `C:` System, `D:` Data)
  - Disk 2: 50 GB (Backups)
- **Features Installed**:
  - Active Directory Domain Services (AD DS)
    - Users: `u1`, `u2`, `u3`
  - DHCP Server
  - WDS (Windows Deployment Services)
  - FSRM (File Server Resource Manager)
    - Quota: 200MB limit for `u1`, `u2`, `u3`
    - Image Copy Restriction for `u1` and `u2`
  - WSB (Windows Server Backup)
    - Scheduled backup at 7:00 AM and 7:00 PM daily to Disk 2
  - IIS Web Server
    - Displays: `Welcome to CDAC`
  - FTP Server
    - URL: `ftp.cdac.local`
  - Data Deduplication
    - Enabled on `D:` drive
- **Shared Folder**:
  - `D:\PUB_SERVER` (Accessible as `\\PARAM\PUBLIC`)

---

### 2. SHAVAK - Member Server (Windows Server 2022)
- **Hostname**: `Shavak`
- **IP**: `192.168.100.101`
- **RAM**: 4 GB
- **Domain Join**: `cdac.local`
- **Additional Domain Controller**: Promoted
- **Shared Folder**:
  - `D:\PUB_MEMBER` (Accessible as `\\PARAM\PUBLIC`)

---

### 3. CLIENT - Windows 10 Machine
- **Hostname**: `Client`
- **IP**: Obtained via DHCP
- **Domain Join**: `cdac.local`
- **User Access**:
  - Only `u1` can log in
  - `u2` and `u3` login restricted
- **Failover Test**:
  - When PARAM is offline, `u2` and `u3` should still be able to log in using SHAVAK (ADC)
- **Website Access**:
  - Access `cdac.local` via browser

---

### 4. MAILSERVER - Exchange Server (Windows Server 2022)
- **Hostname**: `MailServer`
- **IP**: `192.168.100.102`
- **Accessible via**: `mail.cdac.local` from all machines

---

## 🌐 Networking
- **Internal Network**: `192.168.100.0/25`
- **DHCP**: Managed by PARAM
- **DNS**: Managed via AD DS
- **VPN & NLB**: Configured for internal testing
- **Virtual Hosting**: Enabled via IIS

---

## 📂 Shared Folders
Both PARAM and SHAVAK expose a unified public folder:
```sh
\\PARAM\PUBLIC
