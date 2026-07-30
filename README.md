# Lab 02 - Nmap Host Discovery and Service Enumeration

## Objective

The objective of this lab was to learn how to use Nmap to discover live hosts, identify open ports, detect running services, and perform operating system fingerprinting in a controlled virtual lab environment.

## Lab Environment

- Host Machine: Windows 11
- Virtualization Platform: Oracle VirtualBox
- Attacker Machine: Ubuntu 26.04 LTS
- Target Machine: Windows 11 VM
- Network: Internal Network

## Tools Used

- Nmap
- Ubuntu Terminal
- Windows Command Prompt

## Commands Performed

### 1. Host Discovery

```bash
nmap -sn 192.168.100.20
```

Purpose:
- Verify that the target host is online.

---

### 2. Basic Port Scan

```bash
nmap 192.168.100.20
```

Purpose:
- Discover open TCP ports on the target machine.

---

### 3. Service Version Detection

```bash
nmap -sV 192.168.100.20
```

Purpose:
- Identify services running on open ports and determine their versions.

---

### 4. Operating System Detection

```bash
sudo nmap -O 192.168.100.20
```

Purpose:
- Attempt to identify the target operating system using TCP/IP fingerprinting.

---

### 5. Aggressive Scan

```bash
sudo nmap -A 192.168.100.20
```

Purpose:
- Perform OS detection, service detection, default NSE script scanning, and traceroute in a single scan.

## Screenshots

Screenshots of each scan are available in the `screenshots` folder.

## Skills Demonstrated

- Host Discovery
- Port Scanning
- Service Enumeration
- Operating System Fingerprinting
- Network Reconnaissance
- Basic Security Assessment

## Lessons Learned

This lab demonstrated how Nmap can be used to gather information about a target system before conducting further security assessments. I learned how different scan types reveal different levels of information and why selecting the appropriate scan is important during reconnaissance.

## Next Steps

The next lab will focus on packet capture and network traffic analysis using Wireshark.
