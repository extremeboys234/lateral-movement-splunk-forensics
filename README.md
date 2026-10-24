# Lateral Movement Simulation & Log Forensics

## Objective
This project demonstrates a simulated lateral movement attack against a Windows environment and the subsequent forensic analysis used to detect the intrusion. The simulation maps to the MITRE ATT&CK framework, specifically focusing on **Lateral Movement (TA0008)** via SMB/Windows Admin Shares and **Execution (TA0002)** via Service Execution.

## Environment & Tools
* **Attacker Machine:** Kali Linux
* **Target Machine:** Windows OS
* **Attack Tool:** Impacket (`psexec.py`)
* **SIEM / Log Analysis:** Splunk Enterprise, Windows Event Viewer

## Methodology

### 1. Attack Execution
An authenticated lateral movement attack was executed from Kali Linux targeting the Windows machine. Using Impacket's `psexec.py`, a malicious payload was transferred and executed over SMB, granting an interactive system-level command shell.

### 2. Log Acquisition
Following the compromise, event logs were extracted directly from the target machine using Windows Event Viewer. Both the `Security` and `System` logs were exported as `.csv` files to ensure clean parsing and ingestion into Splunk.

### 3. Forensic Analysis in Splunk
The exported logs were ingested into a Splunk Enterprise instance to hunt for specific Indicators of Compromise (IoCs) left behind by the `psexec` execution.

#### Detection Queries & Findings:
* **Network Authentication (Event ID 4624):** 
  ```spl
  source="security.csv" "Event ID"=4624
