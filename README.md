# 🏠 Homelab Setup — Windows Server & Active Directory

A personal homelab built to practice real-world sysadmin and IT support skills including Active Directory administration, DNS/DHCP configuration, Group Policy, and network troubleshooting.

---

## 🖥️ Lab Overview

| Component | Details |
|---|---|
| Hypervisor | VirtualBox (free) |
| Server OS | Windows Server 2022 Evaluation |
| Client OS | Windows 10/11 (VM) |
| Network Mode | Internal Network + NAT |
| Domain Name | `lab.local` |

---

## 🗂️ Lab Architecture

```
[ Host Machine ]
      │
      ├── VM1: Windows Server 2022 (Domain Controller)
      │         - Active Directory Domain Services (AD DS)
      │         - DNS Server
      │         - DHCP Server
      │
      └── VM2: Windows 10 Client
                - Domain-joined to lab.local
                - Used to test GPOs, user login, permissions
```

---

## ⚙️ Setup Steps

### Step 1 — Install VirtualBox
- Download from: https://www.virtualbox.org
- Install with default settings

### Step 2 — Download Windows Server 2022 ISO
- Free 180-day eval: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022

### Step 3 — Create the Server VM
- RAM: 2–4 GB minimum
- Disk: 50 GB (dynamically allocated)
- Network Adapter 1: NAT (internet access)
- Network Adapter 2: Internal Network (named `intnet`)

### Step 4 — Install & Configure Windows Server
1. Boot from ISO, choose **Desktop Experience**
2. Set a strong Administrator password
3. Set static IP on the Internal adapter:
   - IP: `192.168.10.1`
   - Subnet: `255.255.255.0`
   - DNS: `127.0.0.1`

### Step 5 — Promote to Domain Controller
```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName "lab.local"
```
Reboot when prompted.

### Step 6 — Configure DHCP
```powershell
Install-WindowsFeature -Name DHCP -IncludeManagementTools
Add-DhcpServerV4Scope -Name "LabScope" -StartRange 192.168.10.100 -EndRange 192.168.10.200 -SubnetMask 255.255.255.0
Set-DhcpServerV4OptionValue -DnsServer 192.168.10.1 -Router 192.168.10.1
```

### Step 7 — Create the Client VM
- RAM: 2 GB minimum
- Disk: 40 GB
- Network: Internal Network (`intnet`)
- Install Windows 10/11

### Step 8 — Join Client to Domain
1. Set DNS to `192.168.10.1`
2. Go to: System → Rename this PC (Advanced) → Change → Domain: `lab.local`
3. Enter domain admin credentials
4. Reboot

---

## 🔬 Practice Scenarios

| Scenario | Skills Practiced |
|---|---|
| Create & manage AD users/OUs | Active Directory, RSAT |
| Apply and test Group Policy Objects | GPO, security policy |
| Simulate account lockout + unlock | AD troubleshooting |
| Configure shared folders with permissions | NTFS, file sharing |
| Troubleshoot DNS resolution failures | DNS, nslookup |
| Review Event Logs for login failures | Event Viewer, log analysis |
| Reset user passwords via PowerShell | PowerShell, AD automation |

---

## 💡 Key Commands Reference

```powershell
# Create a new AD user
New-ADUser -Name "Jane Doe" -GivenName "Jane" -Surname "Doe" -SamAccountName "jdoe" -UserPrincipalName "jdoe@lab.local" -Path "OU=Staff,DC=lab,DC=local" -AccountPassword (ConvertTo-SecureString "P@ssword1" -AsPlainText -Force) -Enabled $true

# Unlock a locked account
Unlock-ADAccount -Identity "jdoe"

# Reset a password
Set-ADAccountPassword -Identity "jdoe" -NewPassword (ConvertTo-SecureString "NewP@ss1" -AsPlainText -Force) -Reset

# List all users in an OU
Get-ADUser -Filter * -SearchBase "OU=Staff,DC=lab,DC=local" | Select-Object Name, SamAccountName, Enabled

# Check last logon
Get-ADUser -Identity "jdoe" -Properties LastLogonDate | Select-Object Name, LastLogonDate
```

---

## 📚 Resources Used

- [Microsoft Learn — Active Directory](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [VirtualBox Documentation](https://www.virtualbox.org/manual/)
- [Windows Server 2022 Eval Center](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)

---

## 🎯 Goals

- [x] Domain Controller deployed
- [x] Client machine domain-joined
- [x] DHCP and DNS configured
- [ ] Group Policy Objects implemented
- [ ] Shared drive permissions configured
- [ ] Simulate and resolve common Tier 1 AD issues
