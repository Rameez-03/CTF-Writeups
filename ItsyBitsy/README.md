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
1. Open the Elastic Instance under the Target IP address, this takes us to all the traffic. Select the absolute dates from beginning to end of March in 2022.
**Answer:** 1482

**Objective 2:** What is the IP associated with the suspected user in the logs?
1. On the left side of elastic is all the fields of data, within that list will be "Source_IP". Once selected, 2 IP addresses will be posted and the amount of traffic that was produced from them. Since we are locating the one that was Suspected for abnormal behaviour, it will be the IP with minimal traffic that was suspicious
**Answer:** 192.166.65.54

**Objective 3:** The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?
1. By checking the traffic of the suspected user we can identify the name of the binary, or admin name in this case.
**Answer:** bitsadmin

**Objective 4:** The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?
1. This will also be identified within the 2 packets from the suspected user, the name of the site in this case be under the header "host"
**Answer:** pastebin.com

**Objective 5:** What is the full URL of the C2 to which the infected host is connected?
1. The full URL will be a combination of the name of the host site, and the resources within the site to access. This leads to "host+uri"
**Answer:** pastebin.com/yTg0Ah6a

**Objective 6:** A file was accessed on the filesharing site. What is the name of the file accessed?
1. Now having the full URL link to the filesharing site, paste it within a browser and access the site. Here the file name and flag will be located
**Answer:** secret.txt

**Objective 7:** The file contains a secret code with the format THM{_____}.
**Answer:** THM{SECRET__CODE}

---

## Conclusion

This room simulated a realistic SOC investigation into suspected C2 activity using limited telemetry. By analysing HTTP connection logs in Kibana, the investigation identified abnormal outbound traffic from a single HR user, the misuse of a legitimate Windows binary (bitsadmin), and communication with a known filesharing service (Pastebin) acting as a C2 channel. Correlating IPs, hosts, URLs, and accessed content allowed the extraction of key IOCs and confirmation of malicious behaviour. Overall, the exercise reinforced core SOC skills such as log-based triage, identifying living-off-the-land techniques, recognising C2 patterns, and making informed incident response decisions under constrained visibility.



