# ️ Wazuh SIEM Home Lab – SSH Brute Force Detection #

## Overview

This project demonstrates the design and deployment of a functional SIEM home lab using:

- Ubuntu 22.04 (Wazuh Manager + Indexer + Dashboard)
- Kali Linux (Attacker machine)
- Filebeat (Log shipper)
- OpenSearch (Indexer backend)

The lab simulates SSH brute-force attacks and validates end-to-end detection, alert indexing, and visualization.


##  Lab Architecture

Kali Linux (Attacker)
        ↓
Ubuntu (auth.log)
        ↓
Wazuh Manager (Rule Matching)
        ↓
Filebeat (Log Forwarding)
        ↓
Wazuh Indexer (OpenSearch)
        ↓
Wazuh Dashboard (Alert Visualization)



## Attack Simulation

From Kali Linux, SSH brute-force attempts were simulated:

bash
for i in {1..6}; do ssh fakeuser@192.168.64.5; done
his generated authentication failures in /var/log/auth.log.

## Detection & Rules Triggered

Wazuh successfully detected the attack using:

Rule ID	Description
5710	SSH attempt using non-existent user
550A3	PAM login failure
2502	Multiple authentication failures

Mapped to:

MITRE ATT&CK T1110 – Brute Force

Technique: T1110.001 – Password Guessing

## Troubleshooting & Technical Challenges

During deployment, a 401 Unauthorized error occurred between Filebeat and Wazuh Indexer.

##Root Cause:

Incorrect authentication credentials for OpenSearch.

##Resolution:

Reset admin password using securityadmin.sh

Generated bcrypt hash via hash.sh

Updated internal_users.yml

Restarted indexer

Validated connectivity using:

sudo filebeat test output

Authentication confirmed via:

curl -k -u admin:<password> https://127.0.0.1:9200

## Results

Alerts successfully indexed in wazuh-alerts-*

80+ SSH authentication failure events captured

Alerts visible in Discover dashboard

Full log pipeline verified end-to-end

##Security Considerations

All credentials and certificates have been redacted from this repository.

Skills Demonstrated:

SIEM deployment and configuration

Log ingestion pipeline troubleshooting

OpenSearch security configuration

SSH attack simulation

MITRE ATT&CK mapping

Incident detection validation

## Author:

Franck Ulrich Seka
Cybersecurity / SIEM Lab Project
