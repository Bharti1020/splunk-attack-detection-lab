# splunk-attack-detection-lab
"A complete Red vs Blue Team cybersecurity project demonstrating real-time attack detection using Splunk Enterprise"
🛡️ SSH Brute-Force Detection & SIEM Analysis Lab
📖 Project Overview
In this project, I simulated a real-world Cyber Attack using Kali Linux to observe how unauthorized access attempts are recorded in system logs. I then performed a forensic extraction of these logs and ingested them into Splunk Enterprise to build a Security Operations Center (SOC) dashboard for threat visualization and incident response.

🎯 Objectives
Simulate a high-velocity brute-force attack using Hydra.

Perform log exfiltration from a Linux target to a Windows analysis host.

Ingest and parse unstructured data in Splunk using Regular Expressions (Regex).

Create actionable security dashboards to identify Indicators of Compromise (IoCs).

🛠️ Tech Stack & Environment
Attacker: Kali Linux (Hydra)

Target: Ubuntu Server (Linux Syslog / auth.log)

SIEM: Splunk Enterprise

Virtualization: Oracle VirtualBox

Languages: SPL (Splunk Processing Language), Regex, Bash

🚀 The Attack Lifecycle
1. Attack Simulation
I used Hydra to target the SSH service on the victim machine. I utilized a dictionary-based attack targeting the root account with the fasttrack.txt wordlist.

Bash
hydra -l root -P /usr/share/wordlists/fasttrack.txt ssh://[TARGET_IP] -t 4
Result: Over 3,300 failed authentication attempts were generated in minutes.

2. Forensic Log Ingestion
The raw events were stored in /var/log/auth.log. I moved these to Splunk to begin the analysis phase.

🏗️ Technical Challenges & Troubleshooting
This section details the real-world obstacles encountered during the lab and the systematic steps taken to resolve them.

Issue 1: VirtualBox Shared Folder "Permission Denied"
The Problem: When attempting to move auth.log to the shared folder /media/sf_splunkLogs, the directory was unreachable or appeared empty.

The Root Cause: The local Linux user did not belong to the vboxsf group, which owns VirtualBox shared mounts.

The Fix: ```bash
sudo usermod -aG vboxsf $USER

A full system reboot was required to refresh group permissions.

Issue 2: Unstructured Data & Field Extraction
The Problem: Splunk ingested the logs as a single "blob" of text. Dashboard panels for src_ip and user returned "No results found."

The Fix: I utilized Regular Expressions (Regex) within SPL to manually extract the fields.

The Logic:

attacker_ip: Extracted using (?<attacker_ip>\d+\.\d+\.\d+\.\d+)

target_user: Extracted using for (?<target_user>\S+) from

Issue 3: Time Synchronization (Clock Skew)
The Problem: Events were not appearing in the "Last 24 Hours" search due to a time mismatch between the VM and the Host.

The Fix: Adjusted Splunk time filters to "All Time" and synchronized the VM guest additions clock.

📊 Splunk Dashboard & Analysis
I developed a comprehensive security dashboard to visualize the 3,314 events.

Key Metrics Visualized:
Attack Velocity: A timechart showing the massive spike in attempts per minute.

Top Attacking IPs: A bar chart identifying the primary threat source (10.0.2.15).

User Targeting: A pie chart showing the distribution of attempts between root and invalid users.

System Defense: A counter for "Maximum authentication attempts exceeded," proving the system's built-in SSH protections were active.

🏁 Conclusion
This lab provided hands-on experience in the "Full Loop" of security operations: from the initial threat (Attack) to the technical data retrieval (SysAdmin) and final intelligence visualization (SOC Analysis).
