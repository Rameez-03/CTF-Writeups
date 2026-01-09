# Shadow Trace — TryHackMe

**Author:** Rameez Ahmed • **Date:** 2025-12-31 • **Difficulty:** Easy
**IP:** 10.64.145.215 • **Platform:** TryHackMe

---

## TL;DR

This scenario simulates a real-world SOC night shift where a suspicious file triggers EDR alerts on a user’s machine. The task focuses on analysing the file, extracting indicators of compromise (IOCs), and correlating security alerts to determine whether the activity is malicious. The lab reinforces core SOC triage skills, basic malware analysis, and incident response decision-making to prevent further spread.

---

## Environment & Tools

* <Target IP>
* Attacker: Kali Linux (tools used: `snort`, `nano`)
* Notes: This writeup is sanitized for public posting. Replace `REDACTED` with your personal notes if you keep a private copy.

---

## Task 1 - File Analysis

**Objective 1:** What is the architecture of the binary file windows-update.exe?
1.Open up "Pestudio" tool to analyse the windows-update.exe file. the architecture will be under "file > type"
**Answer:** 64-bit

**Objective 2:** What is the hash (sha-256) of the file windows-update.exe?
1.In the same tool and file, the sha-256 hash is under "file > sha256"
**Answer:** B2A88DE3E3BCFAE4A4B38FA36E884C586B5CB2C2C283E71FBA59EFDB9EA64BFC

**Objective 3:** Identify the URL within the file to use it as an IOC
1.In the same tool and file, go to the IOC section in the vertical navbar. All indicators are present here including the URL which is under "string > url-pattern"
**Answer:** http://tryhatme.com/update/security-update.exe

**Objective 4:** With the URL identified, can you spot a domain that can be used as an IOC?
1.To find suspicious domains related to "tryhatme", which is a typosquat of the actually url tryhackme, go into powershell and filter for string similar to it. These can also be found in the "Pestudio" tool under "strings"
```powershell
strings .\windows-update.exe | findstr "tryhatme"
```
**Answer:** responses.tryhatme.com

**Objective 5:** Input the decoded flag from the suspicious domain
1.From the previous output of the domain, a coded flag was also outputted. Decode this flag on the tool "cyberchef"
**Answer:** THM{you_g0t_some_IOCs_friend}

**Objective 6:** What library related to socket communication is loaded by the binary?
1.In the Pestudio tool under the header "libraries", the Windows Socket Library is displayed in the description with its corresponding Library
**Answer:** WS2_32.dll

---

## Task 2 - Alerts Analysis

**Objective 1:** Can you identify the malicious URL from the trigger by the process powershell.exe?
1.Open the site and identify the encoded url in the "command" column of the powershell.exe row. Open up cyberchef and paste the encoded url in the input and the recipe as "decoded base-64"
**Answer:** https://tryhatme.com/dev/main.exe

**Objective 2:** Can you identify the malicious URL from the alert triggered by chrome.exe?
1.Open the site and identify the decoded url, it should be in decimal form so paste in cyberchef and put decimal as the recipe.
**Answer:** https://reallysecureupdate.tryhatme.com/update.exe

**Objective 3:** What's the name of the file saved in the alert triggered by chrome.exe?
1.The file should be in the "command" column of the alert inside the entire message.
**Answer:** test.txt

---

## Conclusion

This room walked through a realistic SOC triage scenario, requiring rapid analysis of a suspicious binary alongside multiple EDR alerts. By statically analysing the file, key indicators of compromise such as hashes, malicious URLs, domains, and loaded libraries were identified, providing strong evidence of malicious intent. Correlating these findings with alert data from PowerShell and browser activity demonstrated how attackers may stage payload delivery and persistence using deceptive update mechanisms. Overall, this exercise reinforces the importance of IOC extraction, alert correlation, and timely analyst decision-making in containing potential malware incidents before they escalate.



