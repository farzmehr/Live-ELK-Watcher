# Network Forensic Analysis Report

This document reports the network forensic analysis of the [pcapng](Files/NetworkFrensics.pcapng) file.

## Time Thieves 
At least two users on the network have been wasting time on YouTube. It seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:

- They have set up an Active Directory network.
- They are constantly watching videos on YouTube.
- Their IP addresses are somewhere in the range 10.6.12.0/24.

1. The domain name of the users' custom site is **`frank-n-ted.com`**
2. The IP address of the Domain Controller (DC) of the AD network is **`10.6.12.12`**
3. The name of the file downloaded to the 10.6.12.203 machine is  **`june11.dll`** and the name of the malware is **`ZLoader malware`**
4. In the VirusTotal the malware is classified as a **trojan**

---

## Vulnerable Windows Machine

1. The following information about the infected Windows machine is found

    - Host name: **`172.16.4.205`**
    - IP address: **`00:59:07:b0:63:a4`**
    - MAC address: **`ROTTERDAM-PC`**
    
2. The username of the Windows user whose computer is infected is **`matthijs.devries`**
3. The two IP addresses used in the actual infection traffic were **`185.243.115.84 and 31.7.62.213`**

---

## Illegal Downloads

1. The following information about the machine with IP address `10.0.0.201` is found
    - MAC address: **`00:16:17:18:66:c8 (Msi_18:66:c8)`**
    - Windows username: **`elmer.blanco`**
    - OS version: **`Windows 10`**

2. The user downloaded the file named **`Betty_Boop_Rhythm_on_the_Reservation.avi.torrent`**

