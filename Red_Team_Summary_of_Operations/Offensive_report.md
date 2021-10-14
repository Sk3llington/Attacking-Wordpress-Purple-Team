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
- ### **Target 1**
  - Open port 22 with SSH 
  - Open port 80 with Apache version 2.4.10
  - Open port 111 with rpcbind
  - Open port 139 with SMB
  - Open port 445 with SMB


- ### **Target 2**
  - Open port 22 with SSH 
  - Open port 80 with Apache version 2.4.10
  - Open port 111 with rpcbind
  - Open port 139 with SMB
  - Open port 445 with SMB

_TODO: Fill out the list below. Include severity, and CVE numbers, if possible._

The following vulnerabilities were identified on **Target 1**:

- ### **Target 1**

  - **Vulnerability**: _WordPress XMLRPC GHOST Scanner_
    - **CVE**: CVE-2015-0235
    - **Severity**: High
    - **Description**: A remote attacker could use this flaw to execute arbitary code with the permissions of the user running the application.
    - **Impact**: Can lead to Remote Code Execution on the victim's machine. 
  - **Vulnerability**: _WordPress XMLRPC DoS_
    - **CVE**: CVE-2014-5266
    - **Severity**: Medium
    - **Description**: No limitation in the number of elements in an XML document, allowing remote attackers to cause a Denial of Service (CPU consumption) via a large document.
    - **Impact**: Can put a website offline.
  - **Vulnerability**: _WordPress XML-RPC Username/Password Login Scanner_
    - **CVE**: CVE-1999-0502
    - **Severity**: High
    - **Description**: Attempts to authenticate against a WordPress-site using default, null or blank username and password combinations.
    - **Impact**: Can lead to Unauthorized Access.
  - **Vulnerability**: _WordPress Pingback Locator_
    - **CVE**: CVE-2013-0235
    - **Description**: Allows remote attackers to send HTTP requests to intranet servers, and conduct port-scanning attacks, by specifying a crafted source URL for a pingback, related to a Server-Side Request Forgery (SSRF) issue.
    - **Impact**: This vulnerability can potentially be used to expose information and compromise a site.
  - **Vulnerability**: _User Enumeration_
    - **CVE**: 
    - **Severity**: Medium
    - **Description**: In a user enumeration attack, an attacker looks for subtle differences in how WordPress responds to specific requests. Depending on the response, the attacker can determine whether a user exists or not. WordPress user enumeration works on every WordPress site by default because of a WordPress feature called permalinks. Permalinks are permanent URLs to individual WordPress posts and pages.
    - **Impact**: This vulnerability can potentially be used to expose information and compromise a site.

![WPscan results](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/wp_scan_result.png)


### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _flag1{b9bbcb33e11b80be759c4e844862482d}_
    - **Exploit Used**
      - _User Enumeration_

**Command run**:

```bash
wpscan -eu --url 192.168.1.110/wordpress/
```

![WPscan users result](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/wp_scan_users_result.png)

  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
