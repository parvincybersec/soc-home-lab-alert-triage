# soc-home-lab-alert-triage
SOC home lab simulating SSH brute force, port scanning, and privilege escalation attacks with Splunk-based detection and alerting
# SOC Home Lab – Alert Triage & Detection Engineering

## Overview
A hands-on SOC home lab built to simulate real-world attack scenarios and detect 
them using Splunk SIEM. Demonstrates end-to-end security monitoring: attack 
simulation → log collection → detection query development → alerting → triage.

## Architecture
- **Attacker:** Kali Linux (NAT Network, 10.0.2.13)
- **Victim:** Ubuntu Server 24.04 LTS (NAT Network, 10.0.2.15)
- **SIEM:** Splunk Enterprise (Windows host)
- **Log forwarding:** Splunk Universal Forwarder monitoring auth.log and syslog

## Attacks Simulated

### 1. SSH Brute Force
Simulated using Hydra against SSH. Detected by correlating failed password 
events by source IP.
- See: `02-ssh-bruteforce/`

### 2. Port Scan (Reconnaissance)
Simulated using Nmap service/version scan. Detected via UFW firewall block 
logs, using distinct port count per source IP as the detection signal.
- See: `03-port-scan/`

### 3. Privilege Escalation Attempt
Simulated via repeated failed `sudo`/`su` attempts. Detected by correlating 
PAM authentication failures targeting root.
- See: `04-privilege-escalation/`

## Skills Demonstrated
- SIEM configuration and log forwarding (Splunk Universal Forwarder)
- SPL query development and field extraction (regex/rex)
- Alert engineering and threshold tuning
- Linux log analysis (auth.log, syslog, UFW)
- Network segmentation (VirtualBox NAT Network)
- Incident triage and reporting

## Detection Queries
See [splunk-queries.md](./splunk-queries.md)

## Full Triage Report
See [triage-report.pdf](./triage-report.pdf)
