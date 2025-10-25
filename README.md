# Suricata Intrusion Detection System (IDS) - Experimental Study

## Context

**This repository contains the materials for a project developed as part of the **Security (Segurança)** course unit at the **Coimbra Institute of Engineering (ISEC)** during the 2024/2025 academic year[cite: 4316]. [cite_start]The project focused on the **experimental study** of Suricata as a network Intrusion Detection System[cite: 4124]. [cite_start]The primary goal was **not just to use the tool, but to understand *how* it works** by exploring theoretical foundations and practical application[cite: 4135, 4124].

## Project Goal

[cite_start]The main objective was to gain practical understanding of Suricata's operation in IDS mode by[cite: 4341]:
* [cite_start]Setting up a simulated network environment (testbed)[cite: 4139].
* [cite_start]Launching various types of network attacks[cite: 4125].
* [cite_start]Analyzing Suricata's detection capabilities, log generation, and rule logic[cite: 4169, 4342].

## Testbed Setup (GNS3)

[cite_start]A simplified corporate network was simulated using GNS3 to test Suricata's effectiveness [cite: 4138, 4351-4352]. [cite_start]The topology included [cite: 4353-4357]:
* [cite_start]An **internal zone** ("green area") with client VPCs (Terminals A1-A4, B1-B3), access switches (Switch2, Switch3), a main router (Router1), and the Suricata IDS machine (Ubuntu VM)[cite: 4354].
* [cite_start]An **attack zone** ("red area") with the attacker machine (Kali Linux VM), its access switch (Switch1), and router (Router3)[cite: 4356].
* [cite_start]An **interconnection zone** linking the internal and attack zones via Router2[cite: 4355].
* [cite_start]A central **SPAN-capable switch** (SPAN-CentralSwitch - Cisco IOU L2 image) mirroring traffic between Router1 (eth0/0) and Router2 (eth0/1) to the Suricata VM (via eth0/3) for passive monitoring [cite: 4357, 4406-4412].

[cite_start]**Key Technologies Used[cite: 4373]:**
* [cite_start]**GNS3:** (v2.2.54) Network simulation[cite: 4375, 4188].
* [cite_start]**VirtualBox / VMware:** VM hosting[cite: 4139, 4376].
* [cite_start]**Cisco IOU Images:** L3 for Routers (`i86bi-linux-l3-adventerprisek9-15.5.2T.bin`), L2 (`i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track`) for the SPAN switch [cite: 4378-4379, 4189, 4190].
* [cite_start]**Operating Systems:** Kali Linux (attacker), Ubuntu (Suricata host) [cite: 4382-4383].
* [cite_start]**Suricata:** Open-source IDS engine installed on Ubuntu machine.[cite: 4338].
* [cite_start]**IP-terms:** Simulating end-user terminals[cite: 4385].

## Simulated Attacks & Rule Analysis

[cite_start]Various attacks were launched from Kali Linux (10.0.1.1) against targets in the internal network (e.g., 192.168.1.2, 10.0.0.1) to test detection[cite: 4125]:

1.  [cite_start]**DoS/DDoS SYN Flood (hping3) [cite: 4458-4462]:**
    * [cite_start]*DDoS Simulation:* Used `--rand-source` [cite: 4464-4465]. [cite_start]Suricata generated alerts based on source IP reputation (e.g., ET DROP Spamhaus DROP List, sid:2400020+) [cite: 4466-4473].
    * [cite_start]*DoS Simulation:* Used the actual Kali IP [cite: 4480-4482]. [cite_start]Suricata logged numerous flow timeouts consistent with SYN floods [cite: 4483-4488].
2.  [cite_start]**SNMP Reconnaissance (snmpwalk) [cite: 4494-4497]:**
    * [cite_start]Used `-v2c -c public` against Router1 (10.0.0.1) [cite: 4500-4504]. [cite_start]Suricata triggered alerts based on the rule `GPL SNMP public access udp` (sid:2101411), which detects UDP traffic to port 161 containing the string "public" [cite: 4506-4512, 4514-4525].
3.  [cite_start]**Port Scanning (nmap) [cite: 4683-4687]:**
    * [cite_start]Used `nmap -sS` (SYN Scan) against Kali (10.0.1.1) from an internal host (192.168.0.2) [cite: 4689-4692]. [cite_start]Suricata generated alerts for `SURICATA TCPv4 invalid checksum` (sid:2200074), often associated with Nmap's evasion techniques [cite: 4694-4702, 4704-4713].
4.  [cite_start]**Telnet Brute Force [cite: 4725-4727]:**
    * [cite_start]Attempted Telnet login to Router1 (10.0.0.1) using default credentials ("admin"/"1234") [cite: 4729-4730]. [cite_start]Suricata detected this using the rule `ET EXPLOIT Zyxel DSL CPE Management Interface Default Credentials (admin)` (sid:2060090), which looks for the specific content "admin|0d 00|1234" in established Telnet sessions (port 23) [cite: 4731-4753].

## Performance Observation

* [cite_start]During high-volume attacks (like SYN Floods), an increase in the CPU usage of the Suricata VM was observed (e.g., from ≈50% baseline to ≈80%) [cite: 4788-4789]. [cite_start]This highlights that performance can be impacted by traffic load, especially on resource-constrained systems [cite: 4777-4783].

## Repository Contents

* [cite_start]`/SEC2425-PRJ-GRP07-2023135147.pdf`: The detailed project lab guide/report **(Note: This document is written in Portuguese)**, covering the testbed setup, experiments, rule analysis, performance observations, and conclusions[cite: 4156].
* [cite_start]`/SEC2425-PRJ-GRP07-2023135147.pptx`: Presentation slides summarizing the project[cite: 4156].

## [cite_start]Authors [cite: 4311-4313, 4131]

* Henrique Dias Neves Simões Ferreira (2023135147)
* João Pedro Vila Pomar (2023140947)
* Rodolfo Miguel de Sousa Belchior Brás Oliveira (2023155660)

