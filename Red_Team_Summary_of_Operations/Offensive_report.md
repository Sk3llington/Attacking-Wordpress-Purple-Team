# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Network scan with Nmap to identify Target 1 IP address:

```bash
$ nmap -sn 192.168.0.0/24 | awk '/Nmap scan/{gsub(/[()]/,"",$NF); print $NF > "nmap_scanned_ips"}'
```
![Identify Machines on Local network](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/identified_machines_on_local_network.png)

192.168.1.1 is our Gateway machine and 192.168.1.90 is the Kali VM used for the attacks. We scan the remaining IP addresses with Nmap to identify services and OS details:

![Services and OS details w/ Nmap](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/network_scan_result.png)

This scan identifies the services below as potential points of entry:
- #### **Target 1**
  - Open port 22 with SSH 
  - Open port 80 with Apache version 2.4.10
  - Open port 111 with rpcbind
  - Open port 139 with SMB
  - Open port 445 with SMB


- #### **Target 2**
  - Open port 22 with SSH 
  - Open port 80 with Apache version 2.4.10
  - Open port 111 with rpcbind
  - Open port 139 with SMB
  - Open port 445 with SMB

_TODO: Fill out the list below. Include severity, and CVE numbers, if possible._

The following vulnerabilities were identified on each target:

- ### **Target 1**
  - **Vulnerability**: _WordPress XML RPC Pingback_
    - **CVE**:
    - **Description**: Can be exploited by a simple POST to a specific file on an affected WordPress server
    - **Impact**: Target internal layers, change configuration on devices
  - **Vulnerability**: _WordPress XMLRPC GHOST Scanner_
    - **CVE**: CVE-2015-0235
    - **Description**: Used to determine hosts vulnerable to the  GHOST vulnerability via a call to the WordPress XMLRPC interface
    - **Impact**: If the target is vulnerable, the system will segfault and return a server error
  - **Vulnerability**: _WordPress XMLRPC DoS_
    - **CVE**: CVE-2014-5266
    - **Description**: WordPress XMLRPC parsing is vulnerable to a XML based denial of service
    - **Impact**:
  - **Vulnerability**: _
    - **CVE**:
    - **Description**:
    - **Impact**:
  - **Vulnerability**: _
    - **CVE**:
    - **Description**:
    - **Impact**:
  - **Vulnerability**: _
    - **CVE**:
    - **Description**:
    - **Impact**:
  - **Vulnerability**: _
    - **CVE**:
    - **Description**:
    - **Impact**:


_TODO: Include vulnerability scan results to prove the identified vulnerabilities._

### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _TODO: Insert `flag1.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
