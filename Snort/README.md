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
1.Run Snort 
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

## Task 5 - Operational Mode 1: Sniffer Mode

**Objective:** Practice the parameter combinations by using the traffic-generator script.
**Answer:** *No Answer Needed*

---

## Task 6 - Operation Mode 2: Packet Logger Mode

**Objective 1:** What is the source port used to connect port 53?
1.Run the default configuration file with ASCII Mode 
```bash
sudo snort -dev -K ASCII -l.
```
2.On another Terminal, Execute Traffic Generator script, choose "Task-6 Exercise" and let it run. Kill the previous process once this is done running.
```bash
sudo ./traffic-generator.sh
```
3.A 145.254.160.237 Folder is created, Change the permissions for the folder and naviagte inside. Display the contents
```bash
sudo chmod 777 145.254.160.237
cd 145.254.160.237
ls
```
**Answer:** *3009*

**Objective 2:** Read the snort.log file with Snort; what is the IP ID of the 10th packet
1. Navigate to "TASK-6", Run snort on the log file to read the 10th packet
```bash
snort -r snort.log.1640048004 -n 10
```
**Answer:** *49313*

**Objective 3:** Read the "snort.log.1640048004" file with Snort; what is the referer of the 4th packet
1.Run snort for 4 packets and change the options so that the referer is displayed
```bash
snort -dvr snort.log.1640048004 -n 4
```
**Answer:** *http://www.ethereal.com/development.html*

**Objective 4:** Read the "snort.log.1640048004" file with Snort; what is the Ack number of the 8th packet
1.Run snort for 8 packets and read the ACK flag for the last one
```bash
snort -r snort.log.1640048004 -n 8
```
**Answer:** *0x38AFFFF3*

**Objective 5:** Read the "snort.log.1640048004" file with Snort; what is the number of the "TCP port 80" packets
1.Run snort on the entire log file and find the number of packets in TCP port 80
```bash
snort -r snort.log.1640048004 
```
**Answer:** *41*

---

## Task 7 - Operation Mode 3: IDS/IPS

**Objective 1:** What is the number of the detected HTTP GET methods
1.Use Snort on the defualt configuration file
```bash
sudo snort -c /etc/snort/snort.conf -A full -l.
```
2.On another Terminal, Execute Traffic Generator script, choose "Task-6 Exercise" and let it run. Kill the previous process once this is done running.
```bash
sudo ./traffic-generator.sh
```
3.Analyse the output of the process that has just been killed
**Answer:** *2*

**Objective 2:** You can practice the rest of the parameters by using the traffic-generator script.
**Answer** *No Answer Needed*

---

## Task 8 - Operation Mode 4: PCAP Investigation

**Objective 1:** What is the number of the generated alerts in the mx-1.pcap file
1.Navigate to the "TASK-8" files where the mx-1.pcap is stored
```bash
cd Desktop/Task-Exercises/Exercise-Files/TASK-8
```
2.Run the command on the file with the defualt configurations
```bash
sudo snort -c /etc/snort/snort.conf -A full -l . -r mx-1.pcap
```
**Answer:** *170*

**Objective 2:** Keep reading the output. How many TCP Segments are Queued in the mx-1.pcap file
1.The Queued TCP Segments are also within the output of the last command, located within the "Stream Statistics"
**Answer:** *18*

**Objective 3:** Keep reading the output.How many "HTTP response headers" were extracted in the mx-1.pcap file
1.The HTTP Response Headers are also within the output of the last command, located within the "HTTP Inspect - encodings"
**Answer:** *3*

**Objective 4:** What is the number of the generated alerts in the mx-1.pcap file with the SECOND Configuration File
1.Run the command on the file with the second configurations, the alerts are located in the "Action Stats" of the output
```bash
sudo snort -c /etc/snort/snortv2.conf -A full -l . -r mx-1.pcap
```
**Answer:** *68*

**Objective 5:** What is the number of the generated alerts in the mx-2.pcap file
1.Run the command on the file with the defualt configurations, the alerts are located in the "Action Stats" of the output
```bash
sudo snort -c /etc/snort/snort.conf -A full -l . -r mx-2.pcap
```
**Answer:** *340*

**Objective 6:** Keep reading the output. What is the number of the detected TCP packets in the mx-2.pcap file
1.The Detected TCP Packets are also within the output of the last command, located within the "Breakdown by Protocol"
**Answer:** *82*

**Objective 7:** What is the number of the generated alerts in the mx-2.pcap file and mx-3.pcap file
1.Run the command on both files with the defualt configurations, the alerts are located in the "Action Stats" of the output
```bash
sudo snort -c /etc/snort/snort.conf -A full -l . --pcap-list="mx-2.pcap mx-3.pcap"
```
**Answer:** *1020*

---

## Task 9 - Snort Rule Structure

**Objective 1:** Use "task9.pcap". Write a rule to filter IP ID "35369" and run it against the given pcap file. What is the request name of the detected packet? You may use this command: "snort -c local.rules -A full -l . -r task9.pcap"

1.Navigate to "TASK-9" Directory
```bash
cd Desktop/Task-Exercises/Exercise-Files/TASK-8
```
2.We need access to the "local.rules" file where all the filter rules are, we need to use elevated permissions in order to access and modify
```bash
sudo nano /etc/snort/rules/local.rules
```
3.We need to write the rules and add it to the file. The objective asks to filter IP ID 35369, therefore we will save it to a file called "alert" with any source or destination IPs and the correct options
```bash
alert ip any any <> any any (msg: "IP ID 35369 Detected"; id:35369; sid:100001; rev:1;)
```
4.Save and exit the file then Run the task9.pcap file with default configurations
```bash
snort -c /etc/snort/snort.conf -A full -l . -r task9.pcap
```
5.The file "alert" is created, concatenate the file and find the ID "35369". Then read the name of the packet
```bash
cat alert
```
**Answer:** *Timestamp Request*

**Objective 2:** Clear the previous alert file and comment out the old rules. Create a rule to filter packets with Syn flag and run it against the given pcap file. What is the number of detected packets?

1.Access the Rules file with elevated privileges like last time then comment out the previous rule. Add the new rule which filters packets with SYN flag, key for this rule is the "flags" option being set to S
```bash
sudo nano /etc/snort/rules/local.rules
##alert ip any any <> any any (msg: "IP ID 35369 Detected"; id:35369; sid:100001; rev:1;)
alert tcp any any <> any any (msg: "SYN Packet Detected"; flags:S; sid:100002; rev:1;)
```
2.Save and exit the file then Run the task9.pcap file with default configurations
```bash
snort -c /etc/snort/snort.conf -A full -l . -r task9.pcap
```
3.Alert file has been created like last time, however since the file is large, use grep to filter for the message we wrote in the rule "SYN Pakcet Detected". This allows us to find the packet easier
```bash
grep "SYN Packet Detected" alert | wc -l
```
**Answer:** *1*

**Objective 3:** Clear the previous alert file and comment out the old rules. Write a rule to filter packets with Push-Ack flags and run it against the given pcap file. What is the number of detected packets?

1.Access the Rules file with elevated privileges like last time then comment out the previous rule. Add the new rule which filters packets with Push-Ack flag, key for this rule is the "flags" option being set to PA
```bash
sudo nano /etc/snort/rules/local.rules
##alert ip any any <> any any (msg: "IP ID 35369 Detected"; id:35369; sid:100001; rev:1;)
##alert tcp any any <> any any (msg: "SYN Packet Detected"; flags:S; sid:100002; rev:1;)
alert tcp any any <> any any (msg: "Push-Ack Packet Detected"; flags:PA; sid:100003; rev:1;)
```
2.Save and exit the file then Run the task9.pcap file with default configurations
```bash
snort -c /etc/snort/snort.conf -A full -l . -r task9.pcap
```
3.Alert file has been created like last time, however since the file is large, use grep to filter for the message we wrote in the rule "Push-Ack Packet Detected". This allows us to find the packet easier
```bash
grep "Push-Ack Packet Detected" alert | wc -l
```
**Answer:** *216*

**Objective 4:** Clear the previous alert file and comment out the old rules. Create a rule to filter UDP packets with the same source and destination IP and run it against the given pcap file. What is the number of packets that show the same source and destination address?

1.Access the Rules file with elevated privileges like last time then comment out the previous rule. Add the new rule which filters UDP Packets with the same IP, key for this rule is enabling "sameip" in the options of the rule
```bash
sudo nano /etc/snort/rules/local.rules
##alert ip any any <> any any (msg: "IP ID 35369 Detected"; id:35369; sid:100001; rev:1;)
##alert tcp any any <> any any (msg: "SYN Packet Detected"; flags:S; sid:100002; rev:1;)
##alert tcp any any <> any any (msg: "Push-Ack Packet Detected"; flags:PA; sid:100003; rev:1;)
alert ip any any <> any any (msg: "Same Source-Destination IP Detected"; sameip; sid:100004; rev:1;)
```
2.Save and exit the file then Run the task9.pcap file with default configurations
```bash
snort -c /etc/snort/snort.conf -A full -l . -r task9.pcap
```
3.Alert file has been created like last time, however since the file is large, use grep to filter to save the results of the same IP packets that are actually valid to another file
```bash
grep -A 5 "Same Source-Destination IP Detected" alert > detailed_alerts.txt

cat detailed_alerts.txt
```
**Answer:** *7*

**Objective 5:** Case Example - An analyst modified an existing rule successfully. Which rule option must the analyst change after the implementation?
**Answer:** *rev*

---

## TASK 10 - Snort2 Operation Logic: Points to Remember

**Objective:** Read the task above.
**Answer:** *No Answer Needed*

---

## Conclusion

This room provided hands-on experience with Snort across its core operational modes, reinforcing how an IDS/IPS functions in both real-time detection and offline traffic analysis. By working through sniffer, packet logger, IDS/IPS, and PCAP investigation modes, the lab demonstrated how Snort interprets network traffic, applies rule logic, and generates actionable alerts. Writing and tuning custom rules highlighted the importance of precise rule options, proper revision control, and understanding protocol flags when detecting specific behaviors. Overall, this exercise emphasizes Snort’s effectiveness as a rule-based detection engine and the critical role of accurate configuration and analysis in network security monitoring.



