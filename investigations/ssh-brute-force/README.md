🛡️ SSH Brute Force Investigation

## Executive Summary

A controlled SSH brute force simulation was performed against an OpenSSH server running on Kali Linux to understand how repeated authentication attempts appear in both network traffic and system logs.

The objective was to identify failed login attempts using Wireshark, tcpdump, and journalctl, then correlate the evidence to determine whether unauthorized access was successful.

---

## Lab Environment

| Component | Value |
|-----------|-------|
| Operating System | Kali Linux |
| Target Service | OpenSSH Server |
| Protocol | SSH |
| Port | 22/TCP |
| Analysis Tools | Wireshark, tcpdump, journalctl |
| Environment | Local Lab |

---

## Investigation Objectives

- Capture SSH traffic on TCP port 22
- Observe the TCP three-way handshake
- Identify failed authentication attempts
- Correlate network packets with SSH logs
- Determine whether the attacker gained access
