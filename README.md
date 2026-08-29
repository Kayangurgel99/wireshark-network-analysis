# wireshark-network-analysis
Network traffic analysis and cleartext credential capture lab using Wireshark and Kali Linux.

# Hands-On Lab: Network Analysis & Cleartext Credential Capture

## Executive Summary
This project demonstrates hands-on experience with network troubleshooting, packet analysis, and security auditing within a virtual lab environment. Using Kali Linux and Metasploitable2, I simulated a real-world scenario where unencrypted network traffic exposed user credentials, highlighting key network security practices, diagnostic workflows, and proper protocol selection.

## Skills Demonstrated
* **Network Fundamentals:** IPv4 Subnetting, TCP/IP Stack, Host-Only Virtual Networking, OSI Layer Analysis.
* **Troubleshooting & Diagnostics:** ICMP Ping Testing, Interface Verification (`ifconfig`), Virtual Adapter Configuration.
* **Packet Analysis:** Wireshark Capture Filters, Display Filters (`ftp`), TCP Stream Reassembly.
* **Security & Mitigation:** Identity Management Risk, Encryption Requirements, Legacy Protocol Mitigation.

## Lab Architecture & Technical Environment
* **Virtualization:** Oracle VirtualBox
* **Attacker Workstation:** Kali Linux (`192.168.56.103`)
* **Target Server:** Metasploitable2 (`192.168.56.102`)
* **Network Adapter:** Host-Only Adapter (Promiscuous Mode: *Allow All*)
* **Software:** Wireshark 4.6.0

---

## Step-by-Step Methodology

### 1. Network Setup & Target Verification
* Verified network configuration on the Metasploitable2 target host using `ifconfig`.
* Confirmed assigned IPv4 address (`192.168.56.102`) on interface `eth0` across the shared Host-Only virtual subnet (`192.168.56.0/24`).

![Target Interface Verification](Screenshot%202026-08-28%20204914.png)

---

### 2. Wireshark Initialization & Capture Setup
* Launched Wireshark 4.6.0 on Kali Linux.
* Selected network interface `eth0` to monitor live host-to-target packet transmission.

![Wireshark Start Screen](Screenshot%202026-08-28%20204924.png)

---

### 3. FTP Service Connection Initiation
* Opened Kali Linux terminal and initiated an interactive FTP session to the target host (`ftp 192.168.56.102`).
* Verified initial service banner response (`220 vsFTPd 2.3.4`).

![FTP Connection Attempt](Screenshot%202026-08-28%20205049.png)

---

### 4. Failed Authentication Test
* Attempted initial login with non-existent account credentials (`labtest1`) to test error handling and logging responses.
* Received expected server rejection error (`530 Login incorrect.`).

![FTP Failed Authentication](Screenshot%202026-08-28%20205132.png)

---

### 5. Successful User Authentication
* Re-initiated FTP session and supplied valid target credentials (`USER msfadmin` / `PASS msfadmin`).
* Observed successful logon confirmation message from the vsFTPd daemon.

![FTP Successful Login](Screenshot%202026-08-28%20205325.png)

---

### 6. Background Traffic Capture
* Maintained active packet capture while commands were executed across the open control channel.
* Recorded un-filtered ambient broadcast and protocol activity on interface `eth0`.

![Unfiltered Traffic Log](Screenshot%202026-08-28%20205402.png)

---

### 7. Layer 2 Ethernet Frame Inspection
* Analyzed raw packet details in Wireshark, expanding Ethernet II headers to inspect MAC addresses and address resolution flags.

![Frame Header Analysis](Screenshot%202026-08-28%20205651.png)

---

### 8. Full Session Command Sequence
* Completed full FTP workflow including command execution (`SYST`, `FEAT`) and graceful session termination (`QUIT`).
* Received server acknowledgment (`221 Goodbye.`).

![Terminal Session Summary](Screenshot%202026-08-28%20205812.png)

---

### 9. Protocol-Specific Display Filtering
* Applied Wireshark display filter `ftp` to isolate control channel traffic from background ambient noise.
* Filtered packet list cleanly organized sequential requests (`USER`, `PASS`, `QUIT`) and server response codes (`331`, `230`, `221`).

![Filtered FTP Traffic View](Screenshot%202026-08-28%20210202.png)

---

### 10. TCP Stream Reassembly & Credential Recovery
* Executed **Follow -> TCP Stream** on Stream 0 to reassemble TCP segments into a full, human-readable transcript.
* Highlighted cleartext client command transmissions in red (`USER msfadmin` and `PASS msfadmin`) alongside server responses in blue.

![TCP Stream Credential Recovery](Screenshot%202026-08-28%20210336.png)

---

## Technical Takeaways & Best Practices
1. **Root Cause of Exposure:** Legacy FTP (Port 21) transmits control channel traffic in clear text without payload encryption, making credentials vulnerable to passive network sniffing.
2. **Help Desk & Enterprise Recommendation:** Advise administrators and end-users to disable unencrypted FTP services and enforce **SFTP (SSH File Transfer Protocol - Port 22)** or **FTPS (Port 990/989)** to secure authentication data in transit.
