# Lab 01 – Building a Virtual Cybersecurity Lab

## Overview

This lab documents the process of building a virtual cybersecurity lab using Oracle VirtualBox. The environment consists of an Ubuntu Linux virtual machine and a Windows virtual machine configured on an internal network (LabNet). The goal was to create a safe environment for practicing penetration testing, network analysis, vulnerability assessment, and other cybersecurity exercises.

---

## Objectives

* Build a virtual cybersecurity lab using Oracle VirtualBox.
* Configure Ubuntu and Windows virtual machines.
* Create an isolated internal network (LabNet).
* Configure static IP addresses for both virtual machines.
* Verify connectivity between the virtual machines.
* Prepare the environment for future cybersecurity labs.

---

## Lab Environment

| Component          | Details                   |
| ------------------ | ------------------------- |
| Hypervisor         | Oracle VirtualBox         |
| Attacker Machine   | Ubuntu Linux              |
| Target Machine     | Windows                   |
| Network Type       | Internal Network (LabNet) |
| Ubuntu IP Address  | 192.168.100.10            |
| Windows IP Address | 192.168.100.20            |

---

## Network Configuration

Both virtual machines were connected to an Internal Network named **LabNet**.

* Ubuntu Static IP: 192.168.100.10
* Windows Static IP: 192.168.100.20
* Subnet Mask: 255.255.255.0

This configuration allows both virtual machines to communicate while remaining isolated from external networks.

---

## Connectivity Test

Connectivity between the Ubuntu and Windows virtual machines was verified successfully using the `ping` command.

Successful communication confirmed that the virtual lab was correctly configured and ready for future cybersecurity exercises.

---

## Challenges Encountered

During the lab, several networking issues were encountered, including:

* Virtual machines could not communicate initially.
* Incorrect network adapter configuration.
* Static IP configuration issues.
* Network connectivity troubleshooting.
* VirtualBox Guest Additions clipboard installation challenges.

Each issue was investigated and resolved through systematic troubleshooting, resulting in a fully functional virtual lab environment.

---

## Skills Demonstrated

* Virtual Machine Deployment
* Virtual Network Configuration
* Static IP Configuration
* Linux Networking
* Windows Network Configuration
* Network Troubleshooting
* Connectivity Testing
* Problem Solving

---

## Lessons Learned

This lab provided practical experience in building and troubleshooting a virtual cybersecurity environment. I gained a better understanding of virtual networking, IP addressing, connectivity testing, and the importance of methodical troubleshooting. The completed lab provides the foundation for future exercises involving Nmap, Wireshark, Windows Event Logs, and Vulnerability Assessments.

---

## Screenshots

The screenshots for this lab are available in the screenshots folder and include:

* VirtualBox Manager
* Ubuntu Virtual Machine Running
* Windows Virtual Machine Running
* Network Adapter Configuration
* Static IP Configuration
* Successful Ping Test
* LabNet Configuration

