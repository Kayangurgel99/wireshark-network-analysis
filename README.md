# wireshark-network-analysis
Network traffic analysis and cleartext credential capture lab using Wireshark and Kali Linux.
# Hands-On Lab: Network Analysis & Cleartext Credential Capture

## Executive Summary
This project demonstrates hands-on experience with network troubleshooting, packet analysis, and security auditing in a virtual lab environment. Using Kali Linux and Metasploitable2, I simulated a scenario where unencrypted network traffic exposed user credentials, highlighting key network security practices and proper protocol selection.

## Skills Demonstrated
* **Network Fundamentals:** IPv4 Subnetting, TCP/IP Stack, Host-Only Virtual Networking, OSI Layer Analysis.
* **Troubleshooting & Diagnostics:** ICMP Ping Testing, Interface Verification (`ifconfig`), Virtual Adapter Configuration.
* **Packet Analysis:** Wireshark Capture Filters, Display Filters (`ftp`), TCP Stream Reassembly.
* **Security & Mitigation:** Identity Management Risk, Encryption Requirements, Legacy Protocol Mitigation.

## Lab Architecture & Environment
* **Virtualization:** Oracle VirtualBox
* **Attacker Workstation:** Kali Linux (`192.168.56.103`)
* **Target Server:** Metasploitable2 (`192.168.56.102`)
* **Network Adapter:** Host-Only Adapter (Promiscuous Mode: *Allow All*)
* **Software:** Wireshark

## Step-by-Step Methodology

### 1. Network Setup & Connectivity Verification
* Configured custom virtual adapters to place both host and target on the same `192.168.56.0/24` subnet.
* Conducted ICMP ping tests from Kali Linux to confirm active two-way communication.

### 2. Traffic Capture & Service Interaction
* Initialized live packet sniffing on interface `eth0` via Wireshark.
* Initiated an unencrypted FTP session (`ftp 192.168.56.102`) from the command terminal.
* Authenticated using standard user credentials (`msfadmin`).
![FTP Cleartext Credentials](Screenshot%202026-08-28%20205325.png)
### 3. Packet Inspection & Credential Recovery
* Applied display filter `ftp` to isolate File Transfer Protocol traffic.
* Performed **Follow TCP Stream** to inspect full-duplex session communication.
* Identified plain-text exposure of `USER msfadmin` and `PASS msfadmin` commands across the wire.

## Technical Takeaways & Best Practices
1. **Root Cause of Exposure:** FTP (Port 21) transmits control channel traffic in clear text without payload encryption, making credentials vulnerable to local eavesdropping.
2. **Remediation:** Advise end-users and administrators to disable standard FTP and mandate **SFTP (SSH File Transfer Protocol - Port 22)** or **FTPS (Port 990/989)** to enforce TLS/SSL encryption for data in transit.
