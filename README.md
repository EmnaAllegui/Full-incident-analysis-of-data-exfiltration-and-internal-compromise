# Full-incident-analysis-of-data-exfiltration-and-internal-compromise

---

## Objective

This project is a full investigation of a compromised internal host through the download of a malware via a malicious URL,leading to data exfiltration.

This hands-on investigation was designed to strengthen understanding of real-world attack chains, forensic analysis, and threat hunting methodologies.

---

## Skills Learned

- Analyzing the PCAP using Suricata and Zeek logs  
- Ability to detect malware behavior and C2 communication patterns  
- Network traffic analysis using NetFlow and Wireshark  
- Extraction and identification of Indicators of Compromise (IOCs)  
- Development of analytical and threat hunting skills  

---

## Tools Used

- Suricata  
- ZUI (Zed User Interface)  
- NetworkMiner  
- nfdump (NetFlow analysis)  
- Wireshark  
- Kali Linux  

---

## Steps

### Phase 1: Detection (Suricata)

- Windows executable download detected over HTTP  
- Fake Microsoft Teams payload activity observed  
- Suspicious TeamViewer-related traffic identified  
- Malicious VBS payload delivery detected  
<img width="945" height="459" alt="image" src="https://github.com/user-attachments/assets/ab1c5c01-e69e-4911-826b-1465c5f1c32e" />

---

### Phase 2: Behavioral Analysis (Zeek)

#### Conn.log Analysis
- 10.1.17.215 → 5.252.153.241 (43 minutes)  
- 10.1.17.215 → 20.10.31.115 (40 minutes)  
- 10.1.17.215 → 45.125.66.252 (25 minutes)
<img width="945" height="469" alt="image" src="https://github.com/user-attachments/assets/e97c0872-689f-4ac0-830c-0ce329914f2f" />


#### HTTP.log Analysis
- PowerShell scripts downloaded  
- TeamViewer-related files transferred  
- Executable malware files observed  am_delta_patch_1.421.1491.0_c8e6042b36d8f357a8e298b6f9f2fcde561c2e02.exe
- Repeated suspicious HTTP requests (404 responses)
<img width="945" height="566" alt="image" src="https://github.com/user-attachments/assets/fe8163f9-5b22-4021-b2c6-fa34bfa4a944" />
 

#### DNS.log Analysis
- New malicious domains were observed analyzed using virus total:
  - google-authenticator.burleson-appliance.net   
  - authenticatoor.org  
  - appointedtimeagriculture.com  
<img width="945" height="468" alt="image" src="https://github.com/user-attachments/assets/ed4db54c-9903-44b0-a58c-6ead43c94e91" />


---

### Phase 3: Forensics (NetworkMiner)
- Compromised account identified:
  - username shutchenson
  - hostname desktop-l8c5gsj
<img width="983" height="203" alt="image" src="https://github.com/user-attachments/assets/e7ce84e4-082f-4188-af4d-15701a113a0f" />

- Malicious files identified:
  - TeamViewer.exe  
  - am_delta_patch_1.exe  
<img width="983" height="177" alt="image" src="https://github.com/user-attachments/assets/4d2cd0a6-8f9f-454b-a4fc-9ab12031257f" />


---

### Phase 4: NetFlow Analysis

- Top talkers identified (internal and external hosts)
  - 10.1.17.215 and 10.1.17.215: internal hosts
  - 45.125.66.32 and 5.252.153.241: external hosts
<img width="945" height="416" alt="image" src="https://github.com/user-attachments/assets/c1565733-2fc8-41e5-bfef-fecbcc29c8ec" /><br>
- Large outbound traffic detected<br>
  •	Approximatively 9.6 Mega bytes were transferred from 45.125.66.32 to 10.1.17.215<br>
  •	Approximatively 6.6 Mega bytes were transferred from 5.252.153.241 to 10.1.17.215<br>
<img width="945" height="300" alt="image" src="https://github.com/user-attachments/assets/62bfaefc-c8b6-4cd0-ad81-c04532ee4083" />
- Suspicious communication with external infrastructure <br> 
- Evidence of potential data exfiltration  
<img width="945" height="565" alt="image" src="https://github.com/user-attachments/assets/60859509-b2b1-4132-9846-6cb493067933" />

---

### Phase 5: Deep Packet Inspection (Wireshark)

- Encrypted communication sessions observed  between 10.1.17.215 and the external server 45.125.66.32  
- Encoded data transfer detected  
- Command and Control (C2) behavior confirmed  
<img width="945" height="569" alt="image" src="https://github.com/user-attachments/assets/a3c35480-f68d-4741-8e60-69accc5e83fe" />

---

## Results / Findings

- Internal host **10.1.17.215** was compromised  
- Malware downlaod  
- C2 communication established with external servers  
- Data exfiltration detected  
- Credentials harvesting of user **shutchenson**  

---

## Indicators of Compromise (IOCs)

### IP Addresses
- 45.125.66.252  
- 5.252.153.241  

### URLs
- http://5.252.153.241/api/file/get-file  
- http://5.232.153.241/1517096937  

### Files
- TeamViewer.exe  
- am_delta_patch_1.exe  

### Ports
- 80  
 
---

## Conclusion

Correlation based on previous analysis indicates that the internal host 10.1.17.215 was likely compromised by a malware downloaded through malicious URL, leading to C2 communication and data exfiltration.
# Recommendations

---

## Immediate Actions

- Isolate the host **10.1.17.215** to prevent further spread  
- Block all incoming and outgoing traffic from the following hosts via firewall policy:  
  - 45.125.66.252  
  - 5.252.153.241  
- Disable the user account **shutchenson**  
- Reset credentials for **shutchenson** account  
- Perform full forensic analysis on the affected system  

---

## Strategic Actions

- Implement Intrusion Detection System (IDS) and Intrusion Prevention System (IPS)  
- Implement Endpoint Detection and Response (EDR) solution  
- Update antivirus software and all outdated applications  
- Conduct user awareness training to recognize phishing URLs  
- Implement Multi-Factor Authentication (2FA) for all critical accounts  
- Deploy and enforce Email Security Gateway  
- Conduct regular threat hunting exercises to detect malicious URL patterns  
- Enforce domain policies using Group Policy Objects (GPO)  
- Implement the principle of least privilege across the environment  

---
