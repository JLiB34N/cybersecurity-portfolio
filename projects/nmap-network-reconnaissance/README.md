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
Configured both virtual machines to use the Host-Only Adapter network mode.
This configuration allows the virtual machines to communicate with each other through an isolated virtual network without exposing the experiment to the physical home network or the broader internet.

### Step 3: Identify the Target IP Address
Started both virtual machines and identified the IP address of Metasploitable 2 using: <b>ifconfig</b>
The target machine was assigned the IP address: 192.168.56.101

### Step 4: Verify Network Connectivity
From the Kali Linux terminal, executed the following command: <b>ping 192.168.56.101</b>
The successful ping responses confirmed that the attacker machine could communicate with the target machine.

### Step 5: Perform Nmap Service Detection
Executed the following command from Kali Linux: <b>nmap -sV 192.168.56.101</b>
The -sV option enables service and version detection. Nmap attempts to identify the services running on open ports and determine their software versions.
The scan results were then analysed to identify potentially vulnerable or unnecessarily exposed services.

## 4. Results and Findings
The Nmap scan identified multiple open TCP ports on the Metasploitable 2 target machine. The following services and potential security concerns were identified:

- Port 21 (ftp): Running vsftpd 2.3.4, indicating an active File Transfer Protocol service. This
specific version contains a well-known backdoor vulnerability.
● Port 22 (ssh): Running OpenSSH 4.7p1. Used for secure remote administration, but an
outdated version may be susceptible to exploitation.
● Port 23 (telnet): Telnet service. It transmits data in plaintext (making it vulnerable to
credential sniffing) and is misconfigured to allow remote login, often accepting default
credentials like msfadmin:msfadmin.
● Port 25 (smtp): Simple Mail Transfer Protocol. Lacks encryption and is misconfigured to
allow unauthorized user enumeration, enabling attackers to verify valid usernames on the
system.
● Port 53 (domain): Domain Name System (DNS) server. Open DNS services can be abused
for reconnaissance (like zone transfers) or leveraged in reflection/amplification DDoS
attacks.
● Port 80 (http): Running Apache httpd 2.2.8. Indicates a web server is active, exposing
various deliberately vulnerable web applications hosted on the machine.
● Port 111 (rpcbind): Remote Procedure Call (RPC) bind service. It maps RPC programs to
network ports. Exposing it can allow attackers to query what RPC services are available,
leading to further enumeration and exploitation.
● Port 139 (netbios-ssn): Samba (SMB) service. Running a vulnerable version of Samba
(3.0.20) which contains the well-known usermap_script vulnerability, allowing for
unauthenticated Remote Code Execution (RCE) as root.
● Port 445 (netbios-ssn): Samba (SMB) service operating over TCP. Susceptible to the same
SMB vulnerabilities as port 139, allowing attackers to read/write files or execute commands.
● Port 512 (exec): rexec (remote execution) service. Part of the legacy "r-services" suite.
Misconfigured to trust all hosts (via a + + entry in the .rhosts file), allowing
unauthenticated remote execution.
● Port 513 (login): rlogin service. Misconfigured with the same .rhosts trust issue,
allowing remote attackers to log in as root without a password.
● Port 514 (shell): rsh (remote shell) service. Similarly misconfigured, allowing remote users
to execute commands without requiring authentication.
● Port 1099 (java-rmi): Java RMI (Remote Method Invocation) Registry. Its default
configuration allows the loading of classes from any remote HTTP URL, which can be
exploited for Remote Code Execution.
● Port 1524 (bindshell): Ingreslock service. On Metasploitable 2, this is a highly vulnerable,
intentionally open bind shell. An attacker can simply use Netcat to connect to this port and
instantly receive a root shell.
● Port 2049 (nfs): Network File System. Configured with weak export settings, allowing an
attacker to remotely mount the server's root file system and access or modify sensitive files.
● Port 2121 (ftp): ProFTPD service. An alternative FTP service running on a non-standard
port. Certain versions of ProFTPD have known vulnerabilities, and it may also be
susceptible to weak credential brute-forcing.
● Port 3306 (mysql): MySQL database service. Misconfigured with weak or blank credentials
for the root user, allowing attackers direct administrative access to the databases.
● Port 5432 (postgresql): PostgreSQL database service. Contains weak default credentials.
Exploitation often allows an attacker to upload UDF (User Defined Function) shared
libraries to execute arbitrary code.
● Port 5900 (vnc): Virtual Network Computing service. Misconfigured with a trivial, easily
brute-forced password (such as password), allowing attackers direct graphical remote
desktop access.
● Port 6000 (x11): X11 window system display server. Left open to the network, allowing
remote attackers to connect, capture keystrokes, take screenshots, or interact with the
graphical session.
● Port 6667 (irc): UnrealIRCd daemon. This specific version contains a malicious backdoor
hidden in the source code, which allows attackers to execute arbitrary commands remotely.
● Port 8009 (ajp13): Apache JServ Protocol (AJP). Used for communication between a web
server and Apache Tomcat. Can be vulnerable to exploits like Ghostcat if unpatched,
allowing local file inclusion or RCE.
● Port 8180 (http): Apache Tomcat manager interface. It is accessible using default
credentials (e.g., tomcat:tomcat), which allows an attacker to upload a malicious WAR
archive file and gain a reverse shell.

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
