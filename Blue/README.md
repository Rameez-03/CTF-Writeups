# Blue — TryHackMe

**Author:** Rameez Ahmed • **Date:** 2025-11-11 • **Difficulty:** Beginner
**IP:** 10.10.9.100 • **Platform:** TryHackMe

---

## TL;DR

A Windows host exploited remotely: a discovered vulnerability was used to achieve remote code execution, gain an initial low-privilege shell as an attacker, and then escalate to SYSTEM. Flags are redacted for public posting.

---

## Environment & Tools

* <Target IP>
* Attacker: Kali Linux (tools used: `nmap`, `msfconsole`)
* Notes: This writeup is sanitized for public posting. Replace `REDACTED` with your personal notes if you keep a private copy.

---

## Recon

**Objective:** discover open services, versions, and likely attack vectors.

1. TCP Port Scan
```bash
nmap -p- <target_ip>
```
This enumerates all TCP ports and reports running services; results will be used to prioritise potential attack vectors

**Objective:** Scan the first 1000 ports

2. TCP Port Scan for first 1000 Ports
```bash
nmap -p 1-999 <target_ip>
```
Ports 1–1000 commonly host services such as HTTP, SMB, and RDP; they are prioritised for follow-up enumeration


**Objective:** Identify the Vulnerabiltiy that the Target Machine has.

From the Hint provided the Vulnerbaility is understood to be related to SMBv1, and with some trial and error of the numerous MS exploits associated the one found to be a match was displayed by
3. Vulnerability Scan on Port 445
```bash
nmap -p 445 --script=smb-vuln-ms17-010 <target_ip>
```
Showing us that the Target is Vulnerable to Remote Code Executuion also known as Eternal Blue, under CVE-2017-0143 family. 

---

## 

---

## 

---

## 

---

## 

---

##

---

## 

---

## 

---

## 
