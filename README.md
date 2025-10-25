# Suricata Intrusion Detection System (IDS) - Experimental Study

## Context

This repository contains the materials for a project developed as part of the **Security (Segurança)** course unit at the **Coimbra Institute of Engineering (ISEC)** during the 2024/2025 academic year. The project focused on the **experimental study** of Suricata as a network Intrusion Detection System. The primary goal was **not just to use the tool, but to understand *how* it works** by exploring theoretical foundations and practical application.

---

## Project Goal

The main objective was to gain practical understanding of Suricata's operation in IDS mode by:
* Setting up a simulated network environment (testbed).
* Launching various types of network attacks.
* Analyzing Suricata's detection capabilities, log generation, and rule logic.

---

## Testbed Setup (GNS3)

A simplified corporate network was simulated using GNS3 to test Suricata's effectiveness. The topology included:
* An **internal zone** ("green area") with client VPCs (Terminals A1-A4, B1-B3), access switches (Switch2, Switch3), a main router (Router1), and the Suricata IDS machine (Ubuntu VM).
* An **attack zone** ("red area") with the attacker machine (Kali Linux VM), its access switch (Switch1), and router (Router3).
* An **interconnection zone** linking the internal and attack zones via Router2.
* A central **SPAN-capable switch** (SPAN-CentralSwitch - Cisco IOU L2 image) mirroring traffic between Router1 (eth0/0) and Router2 (eth0/1) to the Suricata VM (via eth0/3) for passive monitoring.

**Key Technologies Used**:
* **GNS3:** (v2.2.54) Network simulation.
* **VirtualBox / VMware:** VM hosting.
* **Cisco IOU Images:** L3 for Routers (`i86bi-linux-l3-adventerprisek9-15.5.2T.bin`), L2 (`i86bi_linux_l2-ipbasek9-ms.may8-2013-team_track`) for the SPAN switch.
* **Operating Systems:** Kali Linux (attacker), Ubuntu (Suricata host).
* **Suricata:** Open-source IDS engine installed on Ubuntu machine.
* **VPCS:** Simulating end-user terminals.

---

## Simulated Attacks & Rule Analysis

Various attacks were launched from Kali Linux (10.0.1.1) against targets in the internal network (e.g., 192.168.1.2, 10.0.0.1) to test detection:

1.  **DoS/DDoS SYN Flood (hping3)**:
    * *DDoS Simulation:* Used `--rand-source`. Suricata generated alerts based on source IP reputation (e.g., ET DROP Spamhaus DROP List, sid:2400020+).
    * *DoS Simulation:* Used the actual Kali IP. Suricata logged numerous flow timeouts consistent with SYN floods.
2.  **SNMP Reconnaissance (snmpwalk)**:
    * Used `-v2c -c public` against Router1 (10.0.0.1). Suricata triggered alerts based on the rule `GPL SNMP public access udp` (sid:2101411), which detects UDP traffic to port 161 containing the string "public".
3.  **Port Scanning (nmap)**:
    * Used `nmap -sS` (SYN Scan) against Kali (10.0.1.1) from an internal host (192.168.0.2). Suricata generated alerts for `SURICATA TCPv4 invalid checksum` (sid:2200074), often associated with Nmap's evasion techniques.
4.  **Telnet Brute Force**:
    * Attempted Telnet login to Router1 (10.0.0.1) using default credentials ("admin"/"1234"). Suricata detected this using the rule `ET EXPLOIT Zyxel DSL CPE Management Interface Default Credentials (admin)` (sid:2060090), which looks for the specific content "admin|0d 00|1234" in established Telnet sessions (port 23).

---

## Performance Observation

* During high-volume attacks (like SYN Floods), an increase in the CPU usage of the Suricata VM was observed (e.g., from ≈50% baseline to ≈80%). This highlights that performance can be impacted by traffic load, especially on resource-constrained systems.

---

## Repository Contents

* `/SEC2425-PRJ-GRP07-2023135147.pdf`: The detailed project lab guide/report **(Note: This document is written in Portuguese)**, covering the testbed setup, experiments, rule analysis, performance observations, and conclusions.
* `/SEC2425-PRJ-GRP07-2023135147.pptx`: Presentation slides summarizing the project.

---

## Authors

* Henrique Dias Neves Simões Ferreira 
* João Pedro Vila Pomar 
* Rodolfo Miguel de Sousa Belchior Brás Oliveira 
