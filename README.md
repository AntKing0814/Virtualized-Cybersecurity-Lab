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


## 🚀 Key Implementation Steps

### 1. Virtual Environment & Suricata Setup
* Installed Ubuntu Server on VirtualBox.
* Configured Suricata NIDS rules to monitor traffic.

*(Insert screenshot here)*

### 2. SIEM Log Ingestion (Microsoft Sentinel)
* Connected virtual log sources to Azure Log Analytics.
* Created custom telemetry rules and dashboards.

*(Insert screenshot here)*

---

## 📸 Proof of Concept / Screenshots
> Upload your lab screenshots here to demonstrate active alerts and log ingestion.
