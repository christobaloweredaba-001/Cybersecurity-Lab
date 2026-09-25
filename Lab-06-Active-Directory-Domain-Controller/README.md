# Lab 06 — Building an Active Directory Domain Controller

## Objective
Set up a Windows Server as an Active Directory Domain Controller, then join a Windows 10 client to the domain and confirm centralized login works end to end.

## Environment
- VirtualBox 7
- Windows Server 2022 (Desktop Experience) Domain Controller
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
10. Logged into the Windows 10 VM using the domain account (not a local account)
11.  confirming AD authentication was working.

## What I learned

- AD DS is the role that turns a Windows Server into a directory service — but installing the role isn't the same as promoting the server to a Domain Controller; those are two separate steps.
- A forest is the top-level container in AD — since this was my first DC, I created a new one.
- AD relies on DNS to function. When you promote a server to a DC, DNS gets installed alongside it, and every domain-joined client needs to point its DNS settings at the DC to find the domain.
- VirtualBox's default NAT networking isolates VMs from each other. To let two VMs communicate directly (like a real LAN), they need to be on the same Internal Network.
- Internal Network mode has no DHCP server, so IP addresses have to be set manually on each VM.
- Once a client joins a domain, it trusts the DC for authentication, accounts created centrally on the server can log into any domain-joined machine, even though the account was never created on that machine.

## Issues I ran into (and how I fixed them)

- **"No operating system found" on first boot** turned out the boot order had Hard Disk listed before Optical, so the VM tried booting from an empty disk before ever reaching the ISO. Fixed by reordering boot devices (Optical first).
- **Unattended install kept failing** — after multiple attempts, I scrapped the unattended setup and did a manual Windows Server install instead, which worked immediately.
- **Ctrl+Alt+Delete didn't reach the VM** the host OS intercepts that shortcut by default. Used VirtualBox's Input menu → Keyboard → "Insert Ctrl-Alt-Del" instead.
- **@ symbol wouldn't type at the Windows login screen** worked fine everywhere else in the VM, just not on the sign-in screen specifically. Fixed by adjusting the keyboard layout in Windows settings and using the on-screen keyboard as a fallback.

## Next steps
- Create additional OUs and users to build out a more realistic structure
- Apply a Group Policy (e.g. password policy or desktop restrictions) and confirm it applies to the client
- Look at basic AD security hardening and common misconfigurations

- ## Part 2 — Security Groups and Group Policy

After getting the domain and client working, I went further and configured centralized permissions and policy enforcement.

### Security Groups

Created a Security Group (`IT-Support-Team`) inside the IT-Department OU and added my test user to it. This is the standard way permissions are managed in AD — instead of granting access to individual users one at a time, you grant it to a group, and manage membership instead. Add or remove someone from the group, and their access updates automatically.

### Group Policy

Created a Group Policy Object (`Restrict-ControlPanel`) linked to the IT-Department OU, enabling "Prohibit access to Control Panel and PC settings" under User Configuration. Ran `gpupdate /force` on the domain-joined Windows 10 client to pull the policy down, then confirmed Control Panel was actually blocked when logged in as the domain user.

This is the same mechanism real organizations use to enforce security settings, restrict access, and standardize configurations across every machine on a network — configured once, centrally, and pushed out automatically.

### Issues I ran into

- **`gpupdate /force` failed with a clock sync error.** Active Directory (via Kerberos) requires client and server clocks to be within about 5 minutes of each other. Since both VMs are on an isolated internal network with no internet access to auto-correct via NTP, their clocks had drifted apart over time. Fixed by manually syncing the time on both VMs and running `w32tm /resync`.
- **The @ and # symbols wouldn't type at the Windows sign-in screen specifically** — worked fine everywhere else in the VM, including inside the same user's logged-in session. This turned out to be a known VirtualBox quirk with the secure sign-in screen not always forwarding certain keystrokes correctly. Fixed by using the on-screen keyboard (accessibility icon on the sign-in screen) to click the characters instead of typing them.

## What's next
- Explore more Group Policy settings (password policies, login scripts, drive mappings)
- Look into delegation — giving a non-admin user limited rights to manage AD objects
- Basic AD security hardening and common misconfiguration checks
