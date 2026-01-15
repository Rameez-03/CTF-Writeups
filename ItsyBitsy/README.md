# Shadow Trace — TryHackMe

**Author:** Rameez Ahmed • **Date:** 2025-12-31 • **Difficulty:** Medium
**IP:** 10.66.150.102 • **Platform:** TryHackMe

---

## TL;DR

You’re acting as a SOC analyst investigating an IDS alert for possible C2 (command-and-control) traffic from an HR user (Browne). Only HTTP network connection logs are available in Kibana (connection_logs index). Your goal is to analyze the logs, identify the suspicious link and file, extract the malicious content/flag (THM:{…}), and decide if the activity is malicious, practicing real-world SOC triage, IOC extraction, and incident response skills.

---

## Environment & Tools

* <Target IP>
* Attacker: Kali Linux (tools used: `Elastic`, `Kibana`)
* Notes: This writeup is sanitized for public posting. Replace `REDACTED` with your personal notes if you keep a private copy.

---

## Task - Scenario - Investigate a potential C2 communication alert

**Objective 1:** How many events were returned for the month of March 2022?
1.
**Answer:** 1482

**Objective 2:** What is the IP associated with the suspected user in the logs?
1.
**Answer:** 192.166.65.54

**Objective 3:** The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?
1.
**Answer:** bitsadmin

**Objective 4:** The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?
1.
**Answer:** pastebin.com

**Objective 5:** What is the full URL of the C2 to which the infected host is connected?
1.
**Answer:** pastebin.com/yTg0Ah6a

**Objective 6:** A file was accessed on the filesharing site. What is the name of the file accessed?
1.
**Answer:** secret.txt

**Objective 7:** The file contains a secret code with the format THM{_____}.
1.
**Answer:** THM{SECRET__CODE}


---

## Conclusion




