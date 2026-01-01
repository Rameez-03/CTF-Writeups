# Snort — TryHackMe

**Author:** Rameez Ahmed • **Date:** 2025-12-31 • **Difficulty:** Medium
**IP:** 10.64.145.215 • **Platform:** TryHackMe

---

## TL;DR

Snort is an Open Source Intrusion Prevention System using rules to define malicious network activity and find packets matching these rules generating alerts. This lab uses Snort to detect real-time threats, analyse recorded traffic files and identify anomalies. 

---

## Environment & Tools

* <Target IP>
* Attacker: Kali Linux (tools used: `snort`, `nano`)
* Notes: This writeup is sanitized for public posting. Replace `REDACTED` with your personal notes if you keep a private copy.

---

## Task 1 - Introduction

**Objective:** Read the above content, understanding the purpose and ability of snort

**Answer:** *No Answer Needed*

---

## Task 2 - Interactive Material and VM

**Objective:** Navigate to the Task-Exercises folder and run the command "./.easy.sh" and write the output

1. Navigate to the Task-Exercise folder in the Terminal
```bash
cd Desktop/Task-Exercises/
```

2. Run the Command
```bash
./.easy.sh
```

**Answer:** *Too Easy!*

---

## Task 3 - Introduction to IDS/IPS

**Objective 1:** Which IDS or IPS type can help you stop the threats on a local machine?
**Answer:** *HIPS*

**Objective 2:** Which IDS or IPS type can help you detect threats on a local network?
**Answer:** *NIDS*

**Objective 3:** Which IDS or IPS type can help you detect the threats on a local machine?
**Answer:** *HIDS*

**Objective 4:** Which IDS or IPS type can help you stop the threats on a local network?
**Answer:** *NIPS*

**Objective 5:** Which described solution works by detecting anomalies in the network?
**Answer:** *NBA*

**Objective 6:** According to the official description of the snort, what kind of NIPS is it?
**Answer:** *FULL-BLOWN*

**Objective 7:** NBA training period is also known as ...
**Answer:** *BASELINING*

---

## Task 4 - First Interaction with Snort

**Objective 1:** Run the Snort instance and check the build number.
1. Run Snort 
```bash
snort -V
```
**Answer:** *149*

**Objective 2:** Test the current instance with "/etc/snort/snort.conf" file and check how many rules are loaded with the current build.
1.Run the Instance
```bash
sudo snort -c /etc/snort/snort.conf -T
```
`OUTPUT`
```bash
4151 Snort rules read
    3477 detection rules
    0 decoder rules
    0 preprocessor rules
3477 Option Chains linked into 271 Chain Headers
0 Dynamic rules
```
**Answer:** *4151*

**Objective 3:** Test the current instance with "/etc/snort/snortv2.conf" file and check how many rules are loaded with the current build
1.Run the Instance
```bash
sudo snort -c /etc/snort/snortv2.conf -T
```
`OUTPUT`
```bash
Initializing rule chains...
1 Snort rules read
    1 detection rules
    0 decoder rules
    0 preprocessor rules
1 Option Chains linked into 1 Chain Headers
0 Dynamic rules
```
**Answer:** *1*

---

## Flags



---

## Conclusion




