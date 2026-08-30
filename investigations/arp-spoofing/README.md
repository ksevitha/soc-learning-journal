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

## Attack Simulation

A Windows 7 virtual machine acted as the victim, while Kali Linux acted as the attacker. Both systems were connected to the same VMware NAT network.

The attacker enabled IP forwarding and used `arpspoof` to send forged ARP reply packets in both directions:

- Victim (10.0.2.5) → Gateway (10.0.2.1)
- Gateway (10.0.2.1) → Victim (10.0.2.5)

This created a transparent Man-in-the-Middle (MITM) attack while maintaining network connectivity.

## Evidence Collection

### Figure 1 — Baseline ARP Neighbor Table

![ARP Neighbor Table](./screenshots/01-ip-neigh.png)

**Observation:** Kali verified the legitimate gateway mapping before the attack.

---

### Figure 2 — Legitimate ARP Traffic

![Wireshark ARP](./screenshots/02-wireshark-arp.png)

**Observation:** Wireshark captured normal ARP Request and ARP Reply packets.

---

### Figure 3 — IP Forwarding Enabled

![IP Forwarding](./screenshots/03-ip-forwarding.png)

**Observation:** IP forwarding allowed Kali to relay intercepted traffic.

---

### Figure 4 — ARP Spoofing in Progress

![ARP Spoof](./screenshots/04-arpspoof-running.png)

**Observation:** Kali continuously transmitted forged ARP Reply packets.

---

### Figure 5 — Poisoned Windows ARP Cache

![Windows ARP Cache](./screenshots/05-windows-arp-cache.png)

**Observation:** Windows mapped gateway IP `10.0.2.1` to Kali's MAC address, confirming successful ARP cache poisoning.

## Findings

## Investigation Timeline

| Stage | Evidence |
|--------|----------|
| Baseline ARP verified | `ip neigh` |
| ARP traffic captured | Wireshark |
| IP forwarding enabled | `sysctl` |
| Forged ARP replies generated | `arpspoof` |
| Windows ARP cache poisoned | `arp -a` |

---

## Findings

- Successfully executed a controlled ARP spoofing attack in a VMware laboratory.
- Forged ARP Reply packets poisoned the Windows ARP cache.
- The gateway IP (`10.0.2.1`) was redirected to Kali's MAC address.
- IP forwarding allowed Kali to transparently relay packets, creating a functional MITM attack.
- The investigation demonstrates how Layer 2 attacks redirect Ethernet frames while preserving Layer 3 IP routing.

---

## MITRE ATT&CK Mapping

| MITRE Element | Value |
|--------------|-------|
| **Tactic (Why?)** | Credential Access |
| **Technique (How?)** | T1557 – Adversary-in-the-Middle |
| **Procedure (Evidence)** | Forged ARP Reply packets, poisoned Windows ARP cache, and bidirectional traffic interception using `arpspoof`. |

---

## Skills Demonstrated

- ARP protocol analysis
- Wireshark packet capture
- ARP cache investigation
- Man-in-the-Middle (MITM)
- Linux networking (`ip neigh`, `sysctl`)
- ARP spoofing with `arpspoof`
- MITRE ATT&CK mapping
- SOC incident documentation

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Collection | Adversary-in-the-Middle | T1557 |

**Why:** Intercept communication between the victim and the gateway.

**Evidence:** Forged ARP Reply packets in Wireshark, poisoned ARP cache on Windows, and bidirectional ARP spoofing using `arpspoof`.
