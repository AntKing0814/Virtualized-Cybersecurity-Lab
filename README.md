# Virtualized-Cybersecurity-Lab
## 📌 Project Overview
A virtualized cybersecurity home lab designed to test network intrusion detection systems (NIDS), packet analysis, and security rule creation using Oracle VirtualBox, Ubuntu Server, Suricata, and Microsoft Sentinel SIEM.

---

## 🛠️ Tools & Technologies Used
* **Hypervisor:** Oracle VirtualBox
* **Operating Systems:** Ubuntu Server, Windows 10/11
* **Network IDS:** Suricata
* **SIEM / Cloud Security:** Microsoft Sentinel / Azure Log Analytics Workspace
* **Analysis Tools:** Wireshark, Bash

---

## 🏗️ Architecture & Network Setup
​1. Environment & Architecture Overview
* **Virtualization Platform:** Oracle VirtualBox
* **Target Endpoint:** Windows 11 Enterprise (Virtual Machine)
* **SIEM / Backend:** Elastic Cloud SIEM (GCP-hosted deployment, us-east4)
* **Data Collector:** Elastic Agent & Filebeat (Fleet-managed)

2. VM Network Setup: NAT vs. Host-Only Analysis 

For this project, the Windows 11 target VM utilized a NAT (Network Address Translation) configuration for its primary network interface:

​Why NAT? It allows the target VM to seamlessly reach outbound external networks and the Elastic Cloud API endpoints over HTTPS without requiring complex physical router configurations.

​Alternative Considerations:

​Host-Only Network: Restricts traffic entirely to a private switch between the host and guest, which is optimal for completely isolated malware analysis or offline forensics, but blocks cloud SIEM telemetry streaming.
​Bridged Adapter: Places the VM directly onto the physical local network (LAN), which was unnecessary here since the objective was securely tunneling agent data straight to a cloud SIEM instance rather than exposing the VM as an independent LAN node.

​Bridged Adapter: Places the VM directly onto the physical local network (LAN), which was unnecessary here since the objective was securely tunneling agent data straight to a cloud SIEM instance rather than exposing the VM as an independent LAN node.

​3. Host Configuration & Telemetry Engineering Details
​To overcome default Windows logging gaps where command-line arguments are omitted from raw process creation logs, the following configuration steps were applied to the target endpoint:

​A. Advanced Audit Policies (auditpol)
​Enforced strict local auditing controls via PowerShell to ensure process tracking events are actively recorded by the Windows Security Event Log:

auditpol /set /category:"Detailed Tracking" /success:enable

B. Registry-Level Command-Line Auditing
​Configured the Windows Registry to mandate that full command-line arguments accompany process creation tracking (Security Event ID 4688)

* **Registry Path:** HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Audit

* **Key Name:** ProcessCreationIncludeCmdLine_Enabled

* **Value:** 1 (DWORD)

​4. Detection Engineering & Verification

* **Adversary Emulation:** Simulated MITRE ATT&CK Discovery techniques (T1082: System Information Discovery, T1087: Account Discovery) via administrative PowerShell execution (whoami /priv, net user, systeminfo).

* **Threat Hunting (KQL):** Queried normalized Elastic Common Schema (ECS) fields in Elastic Discover to validate telemetry enrichment:

process.name : ("whoami.exe" or "net.exe" or "systeminfo.exe")

Custom Detection Rule: Created and scheduled a custom rule in Elastic Security mapped to the Discovery tactic, verifying real-time alert generation and severity tagging (Medium) upon malicious command execution.


## 🚀 Key Implementation Steps

### 1. Virtual Environment & Suricata Setup
* Installed Ubuntu Server on VirtualBox.
* Configured Suricata NIDS rules to monitor traffic.

<img width="1239" height="845" alt="enabling audit " src="https://github.com/user-attachments/assets/e569491c-3be3-4b3c-a822-68c40af4d84b" />

<img width="987" height="519" alt="recon info" src="https://github.com/user-attachments/assets/ad12e3cd-ad84-441b-9253-4a082f6ff428" />

<img width="1080" height="683" alt="execution of reconnaissance commands " src="https://github.com/user-attachments/assets/df86b6b0-e1e3-465b-a7a3-8d5440e49746" />

<img width="1290" height="812" alt="verified the interface state " src="https://github.com/user-attachments/assets/8d904e16-2b57-4856-9f04-e849a81ae32f" />


### 2. SIEM Log Ingestion (Microsoft Sentinel)
* Connected virtual log sources to Azure Log Analytics.
* Created custom telemetry rules and dashboards.

<img width="1906" height="896" alt="Table of isolated event" src="https://github.com/user-attachments/assets/d0ada407-95d4-4aa8-b7b7-17a668b58b50" />

<img width="1906" height="889" alt="Screenshot 2026-10-07 215928" src="https://github.com/user-attachments/assets/06a5c0d0-b504-4412-9845-d01bc8da062a" />
