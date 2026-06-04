# 🔒 Splunk Enterprise SIEM Security Monitoring Lab

A fully functional Security Information and Event Management (SIEM) home lab engineered to centralize logs, monitor network infrastructure, and catch live cyber attacks. Out of Wazuh, Azure Sentinel, and Splunk, I found Splunk to be amazing because of its GUI, deep customization options, and how easy it is to navigate. Adding my pfSense firewall data to Splunk was also way simpler compared to Wazuh, making it the perfect choice for this project[cite: 1]. 

The lab is built on a virtualized architecture where network traffic and endpoint logs are streamed into a central Splunk instance, allowing for real-time blue-team threat detection and analysis.

---

## 🧰 Technologies & Tools

| Category | Technology |
|---|---|
| SIEM Platform | Splunk Enterprise 10.4.0 |
| Hosting Environment | Ubuntu Server via Linux KVM Hypervisor (Hosted on TrueNAS Scale) |
| Firewall / Router | Netgate pfSense Plus[cite: 1] |
| Log Forwarding | Syslog-ng (pfSense) + Splunk Universal Forwarders (Endpoints) |
| IDS/IPS | Suricata (Integrated with pfSense)[cite: 1] |
| Attacker Platforms | Kali Linux |
| Target Environments | Windows Client + Windows Server Active Directory Domain Controller |
| Attack Tools Used | Nmap, CrackMapExec, Custom Brute-Force Scripts |

---

## 🏗️ Lab Infrastructure & Log Ingestion Setup

The infrastructure focuses on collecting data from different layers of the network to give complete visibility over the environment.

### Splunk Installation on KVM
![Installed_Splunk_KVM](Installed_Splunk_KVM.png)
Splunk Enterprise up and running inside an Ubuntu Server virtual machine, hosted via the Linux KVM hypervisor on my TrueNAS hardware[cite: 1].

### Accessing the SIEM Interface
![Logging_In_Splunk](Logging_In_Splunk.png)
Logging into the Splunk web interface from the management network to verify access to the newly deployed server instance.

### Network Ingestion via pfSense
![Connecting_pfSense_Splunk](Connecting_pfSense_Splunk.png)
Configuring advanced syslog-ng settings on the pfSense firewall. This rule sends all network traffic and system logs directly to the Splunk SIEM destination over UDP port 514[cite: 1].

### Endpoint Forwarder Deployment
![Installing_Forwarder](Installing_Forwarder.png)
Deploying the Splunk Universal Forwarder installer on a Windows client machine to securely collect and send local Windows Event Logs to the main indexer.

### Verification of Connected Devices
![Connected_Devices_To_Splunk](Connected_Devices_To_Splunk.png)
Running an internal search (`index=_internal host=* | stats count by host`) to verify that all four core hosts are successfully forwarding logs to Splunk: the Windows Client, the Active Directory Domain Controller (DC), the Ubuntu Splunk Server, and the target test host.

---

## 🔍 Attack Simulations & Detection Engineering

Once logging was fully set up, I used Kali Linux to run various offensive simulations against the network to see if my custom Splunk alerts would catch them.

### Active Security Alerts Overview
![Custom_Alert_Logs](Custom_Alert_Logs.png)
The main Alerts dashboard showing my three active, custom-engineered rules enabled and monitoring the environment: Brute Force Detection, New Admin Account Created, and Suricata IDS Alert Detected.

### 1. Nmap Scan Detection
![Nmap_Scan_DC](Nmap_Scan_DC.png)
Running an aggressive Nmap scan (`nmap -sS -sV -A`) from a Kali Linux VM against the Active Directory Domain Controller. Splunk monitors the rapid connection attempts to detect the reconnaissance phase before an actual attack happens.

### 2. Brute-Force Attack Logs
![BruteForce_Log](BruteForce_Log.png)
Splunk Security Essentials dashboard catching a massive brute-force attack consisting of 3,509 failed authentication attempts originating from the Kali Linux host machine. The analytics automatically map out the top targeted accounts and source IPs.

### 3. CrackMapExec Password Spraying
![Password_Spray_DC](Password_Spray_DC.png)
Tracking a live password spray simulation against the Domain Controller using CrackMapExec. The logs catch the exact moments the tool attempts to authenticate across multiple network accounts to find a weakness.

### 4. Rogue Local Admin Creation
![New_Admin_Acct_Creation_Detection](New_Admin_Acct_Creation_Detection.png)
A high-severity detection rule tracking unauthorized privilege escalation. When a backdoor administrator account named "hacker" is created and added to the local administrators group via PowerShell, Splunk instantly catches it by correlation filtering for Windows Event Codes 4720 and 4732.

---

## 📊 Security Operations Dashboards & Auditing

To keep things organized, I built dashboards to monitor overall network health and specific security alerts in one place.

### SOC Monitoring Dashboard
![SOC_Dashboard_1](SOC_Dashboard_1.png)
The top section of my custom Security Operations Center (SOC) dashboard. It aggregates real-time data feeds, charts incoming events, and highlights critical security trends across all endpoints.

### Suricata IDS Dashboard Integration
![SOC_Dashboard_2](SOC_Dashboard_2.png)
The lower half of the SOC dashboard, featuring a dedicated panel for Suricata IDS logs. This brings deep network intrusion alerts directly into the same view as the system event logs.

### Raw Suricata IDS Search
![Suricata_Search](Suricata_Search.png)
Diving into the raw Suricata network signature logs inside Splunk to investigate automated network probes, malicious traffic patterns, and dropped packets.

### Splunk User Audit Trail
![audit-trail-dashboard](audit-trail-dashboard.png)
The centralized Audit Trail Dashboard used to monitor internal SIEM health. It tracks active user sessions, login browser user agents, and distinct application activity to ensure no unauthorized changes are made inside Splunk itself.

---

## 🙋 Author

**Nicholas Efstathiou**  
Cybersecurity | Network Engineering | Home Lab  
[LinkedIn](https://www.linkedin.com/in/NickStat23)[cite: 1]
