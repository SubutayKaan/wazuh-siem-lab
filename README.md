# Wazuh SIEM Lab: Threat Detection & Incident Response

## 📌 Purpose
This project demonstrates the deployment of an open-source SIEM (Wazuh) and the simulation of a real-world cyber attack. The goal was to build a Blue Team (defensive) environment, launch a Red Team (offensive) brute-force attack from a separate machine, and analyze the generated alerts using the MITRE ATT&CK framework.

## 🏗️ Architecture
*   **Target / SIEM Server:** Ubuntu Server 24.04 (Running Wazuh Manager, Indexer, and Dashboard)
*   **Attacker Machine:** MacBook Pro (Simulating an external threat actor)
*   **Network:** Bridged Adapter (Both machines on the same local network)

## 🛠️ Tools Used
*   **Wazuh (SIEM/XDR):** For log collection, alerting, and dashboard visualization.
*   **LVM (Logical Volume Manager):** Used to expand disk space when the initial allocation was insufficient.
*   **Nmap & SSH:** For port scanning and brute-force simulation.

## 🚀 Steps Taken
1.  **Deployed Wazuh:** Installed Wazuh Manager, Indexer, and Dashboard on Ubuntu Server using the all-in-one installation script.
2.  **Infrastructure Troubleshooting:** The initial installation failed due to a "No space left on device" error. Solved this by using `lvextend` and `resize2fs` to expand the root filesystem via LVM.
3.  **Log Collection:** Since Wazuh Manager cannot run alongside a Wazuh Agent on the same machine (package conflict), the manager's built-in log collector was configured to monitor local `/var/log/auth.log`.
4.  **Attack Simulation:** Used a local MacBook Pro to launch multiple failed SSH login attempts against the Ubuntu server, simulating a brute-force attack.

## 🔍 Key Findings & Results
Wazuh successfully detected the attack within seconds and generated alerts in the dashboard.
*   **Alert:** Authentication Failure / Brute Force
*   **MITRE ATT&CK Mapping:** `T1110` (Brute Force), `T1110.001` (Password Guessing)
*   **Log Analysis:** Extracted the source IP (`data.srcip`), target username (`data.dstuser`), and the raw log from the event details.

*(See the `images/` folder for dashboard screenshots and alert details).*

## 🧠 Lessons Learned & Challenges Overcome
*   **LVM Disk Management:** Learned how to dynamically expand Ubuntu disk space using `lvextend` and `resize2fs` when the initial 24GB root partition filled up.
*   **Wazuh Architecture:** Realized that Wazuh Manager and Wazuh Agent cannot be installed on the same host due to conflicting packages. For self-monitoring, the Manager's `localfile` configuration must be used.
*   **Red vs. Blue:** Gained practical experience in both launching an attack and analyzing the resulting telemetry from a defender's perspective.
