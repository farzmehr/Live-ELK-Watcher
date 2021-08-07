# Blue Team: Summary of Operations

This document reports the the reaction of the current alerts to the attack attack and suggests new alert for a more robust defensive alert.

## Table of Contents
- Network Topology
  - Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

## Network Topology

The Diagram below depicts the network topology of a subnet of `192.168.1.1/24` including an attacker Kali Linux machine an ELK-Stack monitoring system, and a few Linux Ubuntu web servers. The Kali-Linux machine is used to attack vulnerable machines on the network. 

![Diagram](Images/Diagram.png)

### Description of Targets

The targeted machines are Target 1 (192.168.1.110) and Target 2 (192.168.1.115). Target 1 and 2 are Apache web servers and have SSH enabled, ports 80 and 22 are potential malicous point of entry for attackers. This report only focuses on target 1

## Monitoring the Targets

The following alerts were implemented based on the data recevied from the packetbeat and metricbeat installed on the target machine.

##### Excessive HTTP request

`WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes`

  - **Metric**: top 5 `'http.response.status_code'`
  - **Threshold**: over 400
  - **Vulnerability Mitigated**: Brute force and denial of service (DoS) attacks
  - **Reliability**: The alert is a good criterium to detect DOS and brute force attacks. The alert is triggered by excessive number of `HTTP` request. The threshold depends on the freuency of a website visitor. However, it needs to be tuned frequenlty in case the number visitor of the websites changes. it also can reliably detect brute force attack as the number `HTTP` requests in a brute force attack is very high.

In the current attack this alert was not tirggered as there was no brute forcing over the port 80.
##### HTTP Request Size Monitor

   `WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute`

  - **Metric**: sum of `http.request.bytes`
  - **Threshold**: over 3500 bytes
  - **Vulnerability Mitigated**: Local file inclusion, malicious upload and DoS attack
  - **Reliability**: The alert is suitable for detetion of the uploading any malicous file uploads. However, it can trigger many false alerts including legitimate uploads. It can also detect noisy Nmap scans.
  
In the current attack this alert detected the port scanning with Nmap with the active operating system detection (shown in the snapshot below)

##### CPU Usage Monitor

   `WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes`

  - **Metric**: maximum of `system.process.cpu.total.pct`
  - **Threshold**: 50%
  - **Vulnerability Mitigated**: DoS
  - **Reliability**: The alert is suitable for detection of DoS attacks and virus and advare detection.
  
In the current attack this alert was not tirggered as there was no activity with high CPU usage

##### WordPress Scan

   `WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes`

  - **Packetbeat**: maximum of `system.process.cpu.total.pct`
  - **Threshold**: 20
  - **Vulnerability Mitigated**: Exposure of WordPress Vulnerabilites
  - **Reliability**: The attack is suitable for detection of DoS attacks and virus and advare detection.
  
## Patterns of Traffic & Behavior

One of the most effective alert that can be made to detect thhis type of attack is when a shell is spwaned. Auditctl and the the beat to send the logs to kibana Auditbeat are are highly recommended to be installed on the target to detect the spwn of any tty shell.  

## Suggestions for alert improvement

The following alerts suggested to avoid undetected future similar attacks. and would be triggered 

Based on the vulnerabilies found and reported by peneteration testing
_TODO_:

## System hardening and Mitigation Proposals
The alerts used and suggested above pertains to a specific vulnerability. For each vulnerability identified by the alerts  and suggested by the report of the penetration test. The following hardeining steps are recommended.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, some identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1
  - Implementation of a username and password policy and the use of Multi Factor Authenticaton
  - This would mitigate the brute force attacks and password guessing 
- Implementaion of proper access policy
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- auditctl and auditbeat
  - Install auditbeat
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
