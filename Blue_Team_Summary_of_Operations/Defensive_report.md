# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

- IP Range: 192.168.1.0/24
- Netmask: 255.255.255.0
- Gateway: 192.168.1.1

![Network Diagram](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/Diagrams/Wordpress_purple_team_diagram.drawio.png)

##### Machines


|     IPv4      |  OS        | Hostname |
| ------------- |:----------:|:--------:|
| 192.168.1.90  | Linux      | Kali     |
| 192.168.1.100 | Linux      | ELK      |
| 192.168.1.105 | Linux      | Capstone |
| 192.168.1.110 | Linux      | Target 1 |
| 192.168.1.115 | Linux      | Target 2 |

The following machines were identified on the network:

- Kali
  - **Operating System**: Debian
  - **Purpose**: Attacker VM
  - **IP Address**: 192.168.1.100
- ELK
  - **Operating System**: Ubuntu
  - **Purpose**: Monitoring
  - **IP Address**: 192.168.1.100
- Capstone
  - **Operating System**: Ubuntu
  - **Purpose**: Machine to attack (Attacked during previous engagement)
  - **IP Address**: 192.168.1.105
- Target 1
  - **Operating System**: Debian
  - **Purpose**: Machine to attack
  - **IP Address**: 192.168.1.110
- Target 2
  - **Operating System**: Debian
  - **Purpose**: Machine to attack
  - **IP Address**: 192.168.1.115

### Description of Targets


The target of this attack was: `Target 1` (192.168.1.110).

**Target 1** is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### **Excessive HTTP Errors**

The alert is implemented as follows:
  - **Metric**: Queries packetbeat for Number of HTTP Response Status Code
  - **Threshold**: Above 400 within the last 5 minutes
  - **Vulnerability Mitigated**: Network Scan / Brute Force Attack / Unauthenticated Access
  - **Reliability**: High

![Excessive Http Errors](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/kibana_packetbeat_excessive_http_errors_setup.png)

#### **HTTP Request Size**
The alert is implemented as follows:
  - **Metric**: Queries packetbeat for Size of HTTP requests
  - **Threshold**: Size of requests bytes above 3500 within the last minute
  - **Vulnerability Mitigated**: Unusual Web requests (i.e., compared to regular HTTP requests)
  - **Reliability**: Medium

![HTTP Request Size](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/kibana_packetbeat_request_size_monitor_setup.png)

#### **CPU Usage** 
This alert is implemented as follows:
  - **Metric**: Queries metricbeat for system processes usage of CPU
  - **Threshold**: When CPU Activity exceeds 50%
  - **Vulnerability Mitigated**: Malicious running processes 
  - **Reliability**: High

![CPU Usage](https://github.com/Sk3llington/Attacking-Wordpress-Purple-Team/blob/main/images/kibana_cpu_usage_monitor_setup.png)

_TODO Note: Explain at least 3 alerts. Add more if time allows._

### Suggestions for Going Further (Optional)
_TODO_: 
- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain _how_ to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 2
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 3
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
