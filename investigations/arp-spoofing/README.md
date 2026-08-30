# 🛡️ ARP Spoofing Detection

## Executive Summary

A controlled ARP spoofing simulation was performed in a local laboratory to understand how forged ARP replies redirect network traffic and how the activity can be identified using Wireshark and ARP table analysis.

The objective was to detect malicious ARP behavior, verify changes in MAC address resolution, and document the investigation from a Blue Team perspective.

---

## Lab Environment

| Component | Value |
|-----------|-------|
| Operating System | Kali Linux |
| Protocol | ARP |
| Analysis Tool | Wireshark |
| Command Line Tool | arp / ip neigh |
| Environment | Local Lab |

---

## Investigation Objectives

- Capture ARP request and reply packets
- Identify forged ARP replies
- Verify MAC address changes
- Detect ARP poisoning behavior
- Document findings using MITRE ATT&CK

## Findings

- Successfully performed a controlled ARP spoofing attack within a VMware lab environment.
- Forged ARP reply packets were generated using `arpspoof` and captured in Wireshark.
- The Windows 7 victim updated its ARP cache, mapping gateway IP `10.0.2.1` to the attacker's MAC address.
- Traffic was redirected through the Kali attacker, demonstrating a Man-in-the-Middle (MITM) attack.
- The observed behavior maps to MITRE ATT&CK **T1557 – Adversary-in-the-Middle**.
