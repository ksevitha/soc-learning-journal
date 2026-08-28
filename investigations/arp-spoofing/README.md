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
