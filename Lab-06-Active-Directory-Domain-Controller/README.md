# Lab 06 — Building an Active Directory Domain Controller

## Objective
Set up a Windows Server as an Active Directory Domain Controller, then join a Windows 10 client to the domain and confirm centralized login works end to end.

## Environment
- VirtualBox 7
- Windows Server 2022 (Desktop Experience) — Domain Controller
- Windows 10 — domain-joined client
- Internal Network (labnet), static IPs
- Domain: lab.local

## What I did

1. Created a new VM for Windows Server 2022, dynamically allocated 40GB disk, 2GB RAM.
2. Installed Windows Server manually (skipped unattended install after it kept failing to boot — more on that below).
3. Installed the Active Directory Domain Services (AD DS) role via Server Manager.
4. Promoted the server to a Domain Controller, creating a new forest with domain `lab.local`.
5. Switched both the server and my existing Windows 10 lab VM from NAT to Internal Network, so they could see each other.
6. Assigned static IPs (192.168.10.1 for the server, 192.168.10.2 for the client) and pointed the client's DNS at the server.
7. Confirmed connectivity with a ping between the two VMs.
8. Created an Organizational Unit (IT-Department) and a test user account in Active Directory Users and Computers.
9. Joined the Windows 10 VM to the lab.local domain.
10. Logged into the Windows 10 VM using the domain account (not a local account) — confirming AD authentication was working.

## What I learned

- AD DS is the role that turns a Windows Server into a directory service — but installing the role isn't the same as promoting the server to a Domain Controller; those are two separate steps.
- A forest is the top-level container in AD — since this was my first DC, I created a new one.
- AD relies on DNS to function. When you promote a server to a DC, DNS gets installed alongside it, and every domain-joined client needs to point its DNS settings at the DC to find the domain.
- VirtualBox's default NAT networking isolates VMs from each other. To let two VMs communicate directly (like a real LAN), they need to be on the same Internal Network.
- Internal Network mode has no DHCP server, so IP addresses have to be set manually on each VM.
- Once a client joins a domain, it trusts the DC for authentication — accounts created centrally on the server can log into any domain-joined machine, even though the account was never created on that machine.

## Issues I ran into (and how I fixed them)

- **"No operating system found" on first boot** — turned out the boot order had Hard Disk listed before Optical, so the VM tried booting from an empty disk before ever reaching the ISO. Fixed by reordering boot devices (Optical first).
- **Unattended install kept failing** — after multiple attempts, I scrapped the unattended setup and did a manual Windows Server install instead, which worked immediately.
- **Ctrl+Alt+Delete didn't reach the VM** — the host OS intercepts that shortcut by default. Used VirtualBox's Input menu → Keyboard → "Insert Ctrl-Alt-Del" instead.
- **@ symbol wouldn't type at the Windows login screen** — worked fine everywhere else in the VM, just not on the sign-in screen specifically. Fixed by adjusting the keyboard layout in Windows settings and using the on-screen keyboard as a fallback.

## Next steps
- Create additional OUs and users to build out a more realistic structure
- Apply a Group Policy (e.g. password policy or desktop restrictions) and confirm it applies to the client
- Look at basic AD security hardening and common misconfigurations
