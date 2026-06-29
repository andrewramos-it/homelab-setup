# Homelab Environment

A self-built virtualized lab environment used to develop and practice enterprise IT administration skills, including Active Directory, Group Policy, networking, file services, and system monitoring.

## Overview

This homelab was built to simulate a small business / enterprise IT environment end-to-end. It's used as a hands-on extension of my professional experience in IT support and consulting, allowing me to practice infrastructure administration tasks beyond day-to-day Tier 1-3 troubleshooting.

## Platform

- **Virtualization:** VirtualBox / VMware Workstation
- **Host OS:** Windows
- **Guest VMs:** Windows Server (Domain Controller, File Server), Linux (TrueNAS), Windows client machines

## Environment Components

### Active Directory & Group Policy
- Windows Server VM promoted to Domain Controller running Active Directory Domain Services (ADDS)
- Organizational Units (OUs) structured by role/department to scope policy and delegation
- Group Policy Objects (GPOs) created and linked to test OUs, covering settings such as drive mapping, screen lock policy, and restricted access
- Verified policy application using `gpupdate /force` and `gpresult /r`

### File Services
- Dual file server setup running both Windows Server (File Services role) and TrueNAS/Linux
- Used to compare native Windows file sharing and permissions against a NAS-style Linux file server
- Practiced configuring shared folders, NTFS permissions, and share-level permissions

### Networking
- Internal virtual network configured across VMs to simulate domain-joined client/server communication
- Practiced core network troubleshooting using `ping`, `tracert`, `nslookup`, and `ipconfig /all` between VMs

### Monitoring
- System and performance monitoring handled using built-in Windows tools, including Event Viewer, Task Manager, Resource Monitor, and Performance Monitor
- Used to track VM resource usage and diagnose service failures during testing

## Skills Demonstrated

- Active Directory administration (users, groups, OUs, delegation)
- Group Policy creation, linking, and troubleshooting
- Windows Server file services administration
- Cross-platform file server comparison (Windows vs. Linux/TrueNAS)
- Virtual machine provisioning and management
- Network configuration and troubleshooting in a virtualized environment
- System performance monitoring and root-cause diagnosis

## Why This Project Exists

I built this lab to deepen my hands-on experience with enterprise infrastructure administration, specifically Active Directory, Group Policy, and file services, skills that come up consistently in IT operations, systems administration, and supervisory IT roles. It complements the documentation and support process work shown in my other repositories below.

## Related Projects

- [IT Helpdesk Runbook](https://github.com/andrewramos-it/it-helpdesk-runbook) — Structured Tier 1 troubleshooting documentation
- [Ticketing Workflow Tracker](https://tinyurl.com/ARTicketing) — Mock help desk ticket tracking system

## Contact

**Andrew Ramos**
[LinkedIn](https://linkedin.com/in/andrew-ramos-821a42153) | [GitHub](https://github.com/andrewramos-it)
