Nmap Open Service Port Analysis

Project Overview

This project documents a hands-on cybersecurity lab focused on identifying open ports, enumerating services, detecting operating systems, and evaluating network attack surfaces using Kali Linux and Nmap.
The assessment was performed in a controlled CompTIA Security+ training environment across external, guest, client, and server network segments.

> \*\*Authorized Lab Only:\*\* All scanning and testing were performed in a controlled training environment.

Objectives

Identify open TCP ports
Enumerate running services and versions
Detect target operating systems
Compare external, guest, and internal attack surfaces
Identify network-segmentation weaknesses
Evaluate exposed management services
Recommend security controls

Tools Used

Kali Linux
Nmap
Grep
TCP/IP
OPNsense
FreeBSD
Windows Server

Skills Demonstrated

Network reconnaissance
Port scanning
Service enumeration
OS fingerprinting
Attack-surface analysis
Threat-vector analysis
Network-segmentation assessment
Firewall security
Security hardening
Vulnerability assessment

1. External Network Scan

Target: `203.0.113.1`
```bash
nmap 203.0.113.1 -F -sS -sV -O -Pn -oN border-scan.nmap
```
Command options:
Option	Purpose
`-F`	Scan the 100 most common ports
`-sS`	TCP SYN scan
`-sV`	Service/version detection
`-O`	Operating-system detection
`-Pn`	Skip host discovery
`-oN`	Save output in normal Nmap format
To show open ports:
```bash
grep open border-scan.nmap
```

Finding

The scan identified:
```text
25/tcp open smtp Postfix smtpd
```
The external target exposed SMTP on TCP port 25. Nmap also identified the target as FreeBSD 11.2.

Security Observation

Externally accessible services increase the attack surface and should only be exposed when they support a legitimate business requirement.
![External Nmap Scan](screenshots/02-external-nmap-scan.png)
![External Open Port](screenshots/03-external-open-port.png)
![External OS Detection](screenshots/04-external-os-detection.png)

2. Guest Network Assessment

The Kali workstation was moved to the guest network. DHCP was refreshed with:
```bash
dhclient -r \&\& dhclient
```
The interface configuration was verified with:
```bash
ip a s eth0
```
The workstation received an address in the `192.168.16.0/24` network.
![Guest Network IP](screenshots/05-guest-network-ip.png)

Guest Gateway Findings

The guest gateway was scanned at `192.168.16.254`. The results showed these accessible services:
Port	Service	Detected Technology
25	SMTP	Postfix smtpd
53	DNS	Unbound
80	HTTP	OPNsense
443	HTTPS	OPNsense
8000	HTTP-alt	OPNsense
The service name found on several open ports was OPNsense.
![Guest Service Enumeration](screenshots/06-guest-service-enumeration.png)
Nmap identified the gateway as FreeBSD 11.x, consistent with the OPNsense platform.
![Guest OS Detection](screenshots/07-guest-os-detection.png)

Security Finding

Firewall management services were reachable from the guest network. Guest devices should generally not be able to reach administrative interfaces.

Recommended Remediation

Restrict firewall administration to a dedicated management VLAN
Block guest-network access to management interfaces
Apply firewall ACLs between security zones
Allow administrative access only from authorized systems
Use encrypted administrative connections

3. Internal Network Assessment

The Kali workstation was moved to the client network and the internal server was scanned.
Target: `10.1.16.2`
```bash
nmap 10.1.16.2 -F -sS -sV -O -oN server-scan.nmap
```
To show open services:
```bash
grep open server-scan.nmap
```
![Client Network IP](screenshots/08-client-network-ip.png)

Discovered Internal Services

Port	Service
25	SMTP
80	HTTP
111	RPCBind
135	MSRPC
139	NetBIOS-SSN
143	IMAP
443	HTTPS
445	Microsoft-DS / SMB
587	SMTP
2049	Mount/NFS
3306	MySQL
5357	HTTP
![Internal Server Scan](screenshots/09-internal-server-scan.png)
![Internal Server Services](screenshots/10-internal-server-services.png)
Nmap identified the target operating system as Microsoft Windows Server 2016.
Key Security Findings
Excessive Open Ports
The internal server exposed numerous services. Every unnecessary service creates another possible attack path.
Weak Network Segmentation
The client network was able to discover many services on the server network, indicating that stronger segmentation and firewall filtering may be appropriate.
SMB Exposure
TCP port 445 exposed Microsoft-DS/SMB, a service commonly targeted during lateral movement and ransomware activity.
Database Exposure
TCP port 3306 exposed MySQL. Database services should generally be reachable only from authorized application servers and administrative systems.
OS and Service Enumeration
Nmap successfully identified operating-system and service information that could help an attacker select platform-specific exploits.
Attack Surface Summary
The attack surface identified in the lab included:
SMTP
DNS
HTTP/HTTPS
OPNsense management services
SMB
MSRPC
IMAP
NFS
MySQL
Operating-system information
Recommended Security Controls
Close unnecessary ports
Disable unused services
Implement VLAN segmentation
Configure firewall ACLs
Restrict management interfaces
Limit SMB and RPC access
Restrict database access
Use encrypted protocols
Patch operating systems and applications
Replace unsupported systems
Perform regular vulnerability scanning
Monitor suspicious activity with IDS/IPS and SIEM
Apply least-privilege network access
Commands Used
```bash
# External scan
nmap 203.0.113.1 -F -sS -sV -O -Pn -oN border-scan.nmap

# Display external open ports and OS information
grep open border-scan.nmap
grep OS border-scan.nmap

# Refresh DHCP and verify interface
dhclient -r \&\& dhclient
ip a s eth0

# Guest scan results
grep open guest-scan.nmap
grep OS guest-scan.nmap

# Internal server scan
nmap 10.1.16.2 -F -sS -sV -O -oN server-scan.nmap

# Internal server results
grep open server-scan.nmap
grep OS server-scan.nmap
```
What I Learned
This lab strengthened my practical understanding of Nmap reconnaissance, TCP SYN scanning, service enumeration, OS fingerprinting, firewall exposure, network segmentation, threat vectors, attack-surface reduction, security risk assessment, and remediation planning.
The lab demonstrated that open ports should be evaluated based on business need and that effective security requires both perimeter protection and proper internal segmentation.
Lab Result
CompTIA Security+ Assisted Lab: Finding Open Service Ports  
Score: 12/12 - Successfully Completed
![Lab Completion](screenshots/11-lab-score.png)
Disclaimer
This project was completed for educational purposes in an authorized CompTIA cybersecurity training environment. No unauthorized systems were scanned or tested.
