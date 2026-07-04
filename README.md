Blue Team Internship – Week 1: Wazuh SIEM Setup
---
Lab experience with setup, configuration, and validation of the [Wazuh](https://wazuh.com) SIEM/XDR system as part of the Blue Team internship at ITSimplera. This includes setup from scratch of the manager and onboarding of the endpoint agents on two different operating systems along with verification that all the core detection modules produce alerts properly.

Introduction
---
The objective for the first week of the internship was to make sure the Wazuh system is deployed and producing some telemetry data by verifying all the modules work as designed.

Environment:

| Component           | Purpose                      | Operating System         |
|---------------------|------------------------------|--------------------------|
| `ubuntu24`          | Manager, Indexer, Dashboard  | Ubuntu Server 24.04      |
| `SRV2026`           | Agent                        | Windows 10 Home          |
| `linux_agent`       | Agent                        | Linux Mint 22.3          |

All components were installed and operated as VirtualBox VMs on a local hypervisor host in the same private subnet for the communication between agents and manager.
---

What I Did

1. Manager Installation
- Created an Ubuntu Server 24.04 virtual machine and performed the OS updates (`apt update`, `curl`, `default-jdk`)
- Installed Wazuh components (Manager, Indexer, Dashboard) using the official install script in the all-in-one mode (`wazuh-install.sh -a`)
- Ensured all services (Indexer, Manager, Dashboard) were started properly and checked the dashboard login page by HTTPS

2. Agent Deployment — Windows
- Deployed the new agent using the Deploy new agent wizard in the dashboard that generates the installation script in PowerShell with the Windows package target
- Launched the install script on the Windows 10 virtual machine and ensured the `Wazuh` service was installed and running in the Services and Programs and Features sections
- Confirmed that the agent was deployed and its active state on the manager

3. Agent Deployment – Linux
- Redid agent deployment using DEB amd64 target and got the `wget` installation command
- Installed the .deb package, activated and started the wazuh-agent using `systemctl`
- Encountered and solved a registration conflict while testing against the manager host, by installing on a VM (Linux Mint)
- Both agents were seen as active in Agents module, with one Windows and one Linux Mint

4. Module Validation
After getting two agents reporting into the system, I tested out the various capabilities of Wazuh separately to ensure that it is really capturing any action, not just the installed one:

- File Integrity Monitoring (FIM) – made new, updated, and deleted test files in the monitored directory and ensured that each operation occurred in FIM: Recent events with the proper Rule ID (file added / modified / deleted / integrity checksum changed)
- Vulnerability Detection – checked out the automatically created list of CVEs for the installed programs (OS build, Python, pip) on the Windows endpoint by severity
- Security Configuration Assessment (SCA) – performed the automatic *CIS Microsoft Windows 10 Enterprise Benchmark scan and evaluated the pass/fail results and total compliance score
- Security Events / Log Collection – produced several test events on the Windows agent through `cmd`/PowerShell (creation/deletion of a user account, Wazuh service stop/start, turning Windows Firewall off/on, custom events from Application-log) and ensured that each event was properly parsed, categorized, and prioritized in the Security Events module

5. Documented
- Screenshot of each step in the process (installation, registration, dashboard screens, tests results)
- Issues experienced and how they were solved (explained below)
- Double checked each screenshot with the written report to ensure that the figures and numbers in the report are the same as those shown on the dashboard

---

 Repository Contents
---
| `ITSimplera_BlueTeam_Week1_Report.docx` | Report document |


 Technologies and Tools Used
---
- Security Information and Event Management (SIEM), Extended Detection and Response (XDR): Wazuh 4.14 (Manager, Indexer, Dashboard)
- Operating Systems: Ubuntu Server 24.04, Windows 10 Home, Linux Mint 22.3
- Virtualization: Oracle VirtualBox 7.2
- Frameworks

 Notes
- This is a standalone lab environment meant only for educational purposes; it is not linked to any production environment.
- Screenshots contain redacted sensitive information like passwords and personal IP addresses where applicable.
- The number of alerts or vulnerabilities displayed in this report is from a very limited test session and cannot be taken as representative of a production environment.

 Author
---
 Hafsa Murad — Blue Team Intern, ITSimplera
