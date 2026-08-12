Security Assessment Findings

Finding 1 - Internet-Facing SMTP

Port: 25/TCP  
Service: SMTP / Postfix  
Risk: Medium
Observation: SMTP was accessible from the external network.
Risk: Any Internet-accessible service increases the external attack surface.
Recommendation: Confirm the service is required, keep it patched, restrict unnecessary functionality, and monitor it continuously.

Finding 2 - OPNsense Management Reachable from Guest Network

Ports: 80, 443, 8000  
Service: OPNsense  
Risk: High
Observation: OPNsense services were reachable from the guest network.
Risk: A malicious or compromised guest device could attempt credential attacks, exploit management-interface vulnerabilities, or gather sensitive firewall information.
Recommendation: Restrict management access to a dedicated management VLAN or explicitly authorized administrator systems.

Finding 3 - Excessive Internal Server Exposure

Risk: High
Observation: The internal server exposed SMTP, HTTP/HTTPS, MSRPC, SMB, IMAP, NFS, and MySQL services to the client network.
Risk: Excessive internal connectivity increases the opportunities for lateral movement after compromise of a workstation.
Recommendation: Apply network segmentation and firewall ACLs using least-privilege principles.

Finding 4 - SMB Exposure

Port: 445/TCP  
Service: Microsoft-DS / SMB  
Risk: High
Risk: SMB is frequently abused for lateral movement, credential attacks, and ransomware propagation.
Recommendation: Restrict SMB to approved systems and maintain current security patches.

Finding 5 - Database Exposure

Port: 3306/TCP  
Service: MySQL  
Risk: High
Observation: MySQL was reachable from the client network.
Recommendation: Limit database connectivity to authorized application servers and administrative systems.

Finding 6 - OS and Service Disclosure

Detected systems: FreeBSD / OPNsense and Microsoft Windows Server 2016.
Risk: OS and service fingerprinting can help an attacker identify platform-specific vulnerabilities.
Recommendation: Reduce unnecessary exposure, patch systems, enforce segmentation, and monitor reconnaissance activity.
