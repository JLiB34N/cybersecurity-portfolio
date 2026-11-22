# nmap-network-reconnaissance

## 1. Objective
The objective of this project is to perform network reconnaissance using Nmap against a deliberately vulnerable virtual machine (Metasploitable 2).

The main aim is to identify open ports, detect running services and their versions, and investigate potential security risks associated with exposed network services.

Through this exercise, I aim to understand how attackers use network scanning techniques to gather information about target systems and how system administrators can use the same techniques to identify and reduce potential security vulnerabilities.

## 2. Environment and Tools
- Host Operating System: Windows 11
- Virtualisation Software: Oracle VM VirtualBox
- Attacker Machine: Kali Linux
- Target Machine: Metasploitable 2
- Network Configuration: Host-Only Adapter
- Reconnaissance Tool: Nmap
- Scan Technique: Service and Version Detection (-sV)

## 3. Methodology
The following steps were performed to conduct the network reconnaissance experiment.

### Step 1: Set Up the Virtual Environment
Installed Oracle VM VirtualBox as the hypervisor to create and manage the virtual machines.

Imported two virtual machines:
- Kali Linux, which acts as the attacker machine.
- Metasploitable 2, which acts as the deliberately vulnerable target.

### Step 2: Configure the Network
Configured both virtual machines to use the Host-Only Adapter network mode.<br>

This configuration allows the virtual machines to communicate with each other through an isolated virtual network without exposing the experiment to the physical home network or the broader internet.

### Step 3: Identify the Target IP Address
Started both virtual machines and identified the IP address of Metasploitable 2 using: <b>ifconfig</b><br>

The target machine was assigned the IP address: 192.168.56.101

### Step 4: Verify Network Connectivity
From the Kali Linux terminal, executed the following command: <b>ping 192.168.56.101</b><br>

The successful ping responses confirmed that the attacker machine could communicate with the target machine.

### Step 5: Perform Nmap Service Detection
Executed the following command from Kali Linux: <b>nmap -sV 192.168.56.101</b><br>

The -sV option enables service and version detection. Nmap attempts to identify the services running on open ports and determine their software versions.<br>

The scan results were then analysed to identify potentially vulnerable or unnecessarily exposed services.

## 4. Results and Findings
The Nmap scan identified multiple open TCP ports on the Metasploitable 2 target machine. The following services and potential security concerns were identified:

| Port | Service | Version/Finding | Potential Security Concern |
| --- | --- | --- | --- |
| 21 | FTP | vsftpd 2.3.4 | Known backdoor vulnerability associated with this version |
| 22 | SSH | OpenSSH 4.7p1 | Outdated software may contain known security vulnerabilities |
| 23 | Telnet | telnet | Transmits credentials and data without encryption |
| 25 | SMTP | smtp | Potential information disclosure and user enumeration |
| 53 | DNS | Domain | Potential DNS misconfiguration and zone transfer risks |
| 80 | HTTP | Apache httpd 2.2.8 | Outdated web server and potentially vulnerable web applications |
| 111 | RPCbind | RPCbind | Exposes information about available RPC services |
| 139 | NetBIOS/SMB | Samba 3.0.20 | Associated with the usermap_script vulnerability |
| 445 | SMB | Samba | Potential exposure to SMB-related vulnerabilities |
| 512 | Rexec | Remote execution | Legacy remote execution service with potential trust misconfiguration |
| 513 | Rlogin | Remote login | Legacy remote login service with potential authentication weaknesses |
| 514 | Rsh | Remote shell | Legacy remote command execution service |
| 1099 | Java RMI | Java RMI Registry | Potential remote code execution risk if insecurely configured |
| 1524 | Bind Shell | Ingreslock | Intentionally exposed shell service in Metasploitable 2 |
| 2049 | NFS | Network File System | Potential unauthorised file access due to weak export permissions |
| 2121 | FTP | ProFTPD | Additional FTP service with potential version-related vulnerabilities |
| 3306 | MySQL | MySQL | Potential database exposure and weak authentication |
| 5432 | PostgreSQL | PostgreSQL | Potential database exposure and authentication weaknesses |
| 5900 | VNC | Virtual Network Computing | Potential unauthorised remote desktop access |
| 6000 | X11 | X Window System | Potential unauthorised access to graphical sessions |
| 6667 | IRC | UnrealIRCd | Known backdoor vulnerability associated with certain versions |
| 8009 | AJP13 | Apache JServ Protocol | Potential Ghostcat vulnerability if affected version and configuration are present |
| 8180 | HTTP | Apache Tomcat Manager | Potential unauthorised administrative access due to weak credentials |

Screenshots and supporting evidence of the experiment are available in the evidence/ directory.

## 5. Security Implications
The scan revealed that Metasploitable 2 exposes numerous network services, several of which are associated with known vulnerabilities or potentially insecure configurations.

From an attacker's perspective, these services provide valuable information about the target system. By identifying open ports, software versions and running services, an attacker can narrow down potential attack vectors and select services for further investigation.

From a defensive security perspective, this experiment demonstrates the importance of reducing the attack surface.

The following security measures are recommended:
1. Disable unnecessary services: Services that are not required should be stopped to reduce the number of exposed entry points.
2. Update software: Operating systems and network services should be regularly patched to address known vulnerabilities.
3. Use secure protocols: Replace insecure protocols such as Telnet and legacy FTP configurations with secure alternatives where appropriate.
4. Strengthen authentication: Enforce strong passwords and avoid default credentials.
5. Configure firewalls: Restrict access to sensitive services, particularly database and remote administration ports.
6. Perform regular security assessments: Use network scanning tools such as Nmap to identify unexpected open ports and services.

## 6. Challenges Encountered
Initially, I could not establish network connectivity between the two virtual machines.

After investigating the issue, I discovered that the network adapters were not configured correctly for communication between the virtual machines.

I resolved the problem by changing the network adapter settings of both Kali Linux and Metasploitable 2 to Host-Only Adapter.

After applying the changes, I performed another connectivity test using the ping command. The successful responses confirmed that both virtual machines could communicate within the isolated network.

This challenge helped me understand the importance of virtual network configuration when conducting cybersecurity experiments.

## 7. Improvements and Future Work
Although the Nmap scan successfully identified numerous open ports and running services, further investigation is required to determine which services are genuinely vulnerable.

For future work, I plan to:
1. Perform more detailed service enumeration to gather additional information about the identified services.
2. Investigate known vulnerabilities associated with the detected software versions.
3. Validate selected vulnerabilities in the isolated Metasploitable 2 environment.
4. Examine the differences between open, closed and filtered ports.
5. Configure firewall rules and perform additional Nmap scans to observe how network filtering affects the scan results.
6. Compare the results before and after applying security improvements.

These additional experiments would provide a better understanding of how network reconnaissance can support both offensive and defensive cybersecurity activities.

## 8. Conclusion
This project provided practical experience in using Nmap for network reconnaissance and service enumeration.

By scanning Metasploitable 2, I successfully identified multiple open ports and the services running on them. The results demonstrated how network scanning can reveal information about a target system, including its exposed services and potentially outdated software.

One of the key lessons I learned is that exposed ports can increase a system's attack surface. Therefore, system administrators should regularly monitor network services, disable unnecessary ports, update vulnerable software and enforce appropriate access controls.

Overall, this experiment improved my understanding of network reconnaissance and demonstrated the importance of identifying and managing exposed network services as part of a cybersecurity assessment.
