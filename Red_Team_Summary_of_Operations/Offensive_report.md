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


The following vulnerabilities were identified on **Target 1**:

- ### **Target 1**

  - **Vulnerability**: _WordPress XMLRPC GHOST Scanner_
    - **CVE**: CVE-2015-0235
    - **Severity**: High
    - **Description**: A remote attacker could use this flaw to execute arbitrary code with the permissions of the user running the application.
    - **Impact**: This can lead to Remote Code Execution on the victim's machine. 
  - **Vulnerability**: _WordPress XMLRPC DoS_
    - **CVE**: CVE-2014-5266
    - **Severity**: Medium
    - **Description**: No limitation in the number of elements in an XML document, allowing remote attackers to cause a Denial of Service (CPU consumption) via a large document.
    - **Impact**: Can take a website offline.
  - **Vulnerability**: _WordPress XML-RPC Username/Password Login Scanner_
    - **CVE**: CVE-1999-0502
    - **Severity**: High
    - **Description**: Attempts to authenticate against a WordPress-site using the default, null, or blank username and password combinations.
    - **Impact**: This can lead to Unauthorized Access.
  - **Vulnerability**: _WordPress Pingback Locator_
    - **CVE**: CVE-2013-0235
    - **Description**: Allows remote attackers to send HTTP requests to intranet servers, and conduct port-scanning attacks, by specifying a crafted source URL for a pingback, related to a Server-Side Request Forgery (SSRF) issue.
    - **Impact**: This vulnerability can potentially be used to expose information and compromise a site.
  - **Vulnerability**: _User Enumeration_
    - **CVE**: 
    - **Severity**: Medium
    - **Description**: An attacker can determine whether a user exists or not. WordPress user enumeration works on every WordPress site by default because of a WordPress feature called permalinks. Permalinks are permanent URLs to individual WordPress posts and pages.
    - **Impact**: This vulnerability can potentially be used to expose information and compromise a site.
  - **Vulnerability**: _Unprotected Access to ???wp-config.php???_
    - **CVE**:
    - **Severity**: High
    - **Description**: Unauthorized access to this file leads to the leak of all the necessary information required to access the database, such as usernames, passwords, secret keys, etc.
    - **Impact**: This vulnerability is used to further compromise a system, allowing an attacker to gain access to critical data and move laterally within the network/system.
  - **Vulnerability**: _Absence of Data-At-Rest Encryption_
    - **CVE**:
    - **Severity**: High
    - **Description**: Usernames, passwords hashes, and secret keys are directly accessible in plaintext form.
    - **Impact**: An attacker can have direct access to critical ready-to-use data, and password hashes that can potentially be cracked.
  - **Vulnerability**: _Weak Password Policy_
    - **CVE**:
    - **Severity**: High
    - **Description**: Passwords are too short and not complex enough to resist a dictionary or rainbow table attack.
    - **Impact**: An attacker can gain unauthorized access to networks and systems and/or escalate privileges once a system is compromised.
  - **Vulnerability**: _Privilege Escalation_
    - **CVE**:
    - **Severity**: High
    - **Description**:An attacker can use compromised credentials or vulnerabilities in a system to obtain higher privileges allowing greater control over a network or system.
    - **Impact**: Allow an attacker to move laterally within a network or system and access critical data, resulting in the theft of the data, rogue encrypting, and/or its ransoming.

![WPscan results](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/wp_scan_result.png)


### Exploitation


The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _flag1{b9bbcb33e11b80be759c4e844862482d}_
    - **Exploit Used**
      - _User Enumeration & Weak Password Policy_

**Command run**:

```bash
wpscan -eu --url 192.168.1.110/wordpress/
```

![WPscan users result](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/wp_scan_users_result.png)

I decided to try to guess steven's and michael's passwords by trying the most common one and connect via SSH. After a couple of attempts, I figured out michael's password: "michael", and was able to SSH into **Target** 1 as "michael". Since we know a WordPress website is running, I move to the website root directory.

Next, I used the following command to find the first flag:

```bash
grep -re flag1 /var/www/html/
```

![Flag 1](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/finding_flag_1.png)


  - `flag2.txt`: _flag2{fc3fd58dcdad9ab23faca6e9a36e581c}_
      - **Exploit Used**
        - _User Enumeration & Weak Password Policy_
    
Next, I used the following command to find the second flag:

```bash
grep -re flag2 /var/www/
```

![Flag 2](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/finding_flag_2.png)


  - `flag3.txt`: _flag3{afc01ab56b50591e7dccf93122770cd2}_
      - **Exploit Used**
        - _Unprotected Access to `wp-config.php` and Absence of Data-at-Rest Encryption_



Next, we need to locate a MySQL database password. I decide to explore _/var/www/html_ and focus my search on the file `wp-config.php`. This file is a core WordPress file that contains information about the database, including the name, host, username, and password.

I found the file located in _/html/wordpress/_ and run a `cat` command to read the file and found the database username and password, i.e., username: `root` and password: `R@v3nSecurity`

![MySQL database password](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/MySQL_database_password.png)


Next, we access the database using the compromised credentials:


![Successful MySQL login](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/successful_MySQL_login.png)


Now that we are connected to the database, we start by listing all databases with the following command:

```
show databases;
```

![show databases](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/show_databses.png)


Next, we switch to the WordPress database and list the available tables:


![switch to wordpress table and list](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/switch_to_wordpress_table_and_list.png)


Next, we use the following command to list data about each user in the "wp_users" table, successfully getting hashes for users steven and michael:


```
SELECT * FROM wp_users;
```

![listing all users data](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/listing_all_users_data.png)


Next, I saved steven and michael hashes into a .txt file to try to crack them later:


![txt file with hashes](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/txt_file_with_hashes.png)


Next, I explored the database further and found 2 additional flags, flag 3 & flag 4, running the following command:


```
SELECT * FROM wp_posts;
```


![flag3 and flag4 found](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/flag3_and_flag4_found_database.png)


  - `flag4.txt`: _flag4{715dea6c055b9fe3337544932f2941ce}_
      - **Exploit Used**
        - _Weak Password Policy & Privilege Escalation_


Next, I run `John` to try to crack steven and michael's passwords, and I was able to obtain steven's password: `pink84`


![cracking steven's password](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/cracking_steven_password.png)


![cracking steven's password](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/cracking_steven_password_2.png)


Next, I use steven's credentials to SSH into the target:


![ssh as steven](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/ssh_as_steven_target.png)



Once logged in as ???steven??? we try to switch users to root using the `su` command but are unable to do it because of the absence of a controlling terminal (TTY). We use the following command to bypass this issue and obtain root access and discovering the last flag: 


```
$ sudo python -c 'import pty;pty.spawn("/bin/bash");'
```

![gained flag 4](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/gained_root_flag4.png)
