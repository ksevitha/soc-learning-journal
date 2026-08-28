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

---

## Attack Simulation

The attack was performed in a controlled local laboratory to generate both successful and failed SSH authentication events for analysis.

### Commands Executed

**1. Verify SSH service**

```bash
sudo systemctl status ssh
```

**2. Confirm SSH is listening on port 22**

```bash
sudo ss -lntp | grep ':22'
```

**3. Attempt SSH login**

```bash
ssh brutelab@127.0.0.1
```

### Expected Result

- SSH connection established over TCP port 22
- Failed password attempts recorded
- Authentication events logged by OpenSSH

---

# Evidence Collection

## Figure 1 — OpenSSH Service Verification

![SSH Service](screenshots/01-systemctl.png)

**Observation:** The OpenSSH service is active and running, confirming that the SSH server is available to accept connections on TCP port 22.

---

## Figure 2 — Failed SSH Authentication

![Failed SSH Login](screenshots/02-failed-login.png)

**Observation:** An SSH login attempt was made using incorrect credentials. The server rejected authentication and returned **Permission denied**, demonstrating failed password attempts.

---

## Figure 3 — Wireshark Packet Analysis

![Wireshark TCP Port 22](screenshots/03-wireshark-tcp22.png)

**Observation:** Wireshark captured the complete TCP three-way handshake (**SYN → SYN/ACK → ACK**) followed by the SSH protocol exchange on TCP port 22.

---

## Figure 4 — Failed Password Events

![Journalctl Failed Password](screenshots/04-journalctl-failed.png)

**Observation:** `journalctl` recorded multiple **Failed password** events for user `brutelab` originating from `127.0.0.1`, confirming unsuccessful authentication attempts.

---

## Figure 5 — Successful Authentication

![Journalctl Successful Login](screenshots/05-journalctl-success.png)

**Observation:** A subsequent SSH login using the correct credentials generated an **Accepted password** event, confirming successful local authentication from `127.0.0.1`.

---

# Investigation Timeline

| Stage | Evidence |
|--------|----------|
| SSH service verified | `systemctl status ssh` |
| Failed login generated | `ssh brutelab@127.0.0.1` |
| Network traffic captured | Wireshark (`tcp.port == 22`) |
| Failed authentication confirmed | `journalctl` |
| Successful login verified | `journalctl` |

---

# MITRE ATT&CK Mapping

| MITRE Element | Value |
|--------------|-------|
| **Tactic (Why?)** | Credential Access |
| **Technique (How?)** | T1110 — Brute Force |
| **Procedure (Evidence)** | Repeated failed SSH authentication attempts observed in `journalctl` and corresponding SSH traffic captured in Wireshark. |

---

# Findings

- OpenSSH was successfully running on **TCP port 22**.
- Failed SSH authentication attempts were successfully generated and detected.
- Wireshark confirmed the TCP three-way handshake and SSH protocol communication.
- `journalctl` correlated both failed and successful authentication events with the captured network traffic.
- The investigation demonstrates how network packets and system logs can be correlated during SSH authentication analysis in a SOC environment.

---

# Skills Demonstrated

- Wireshark packet analysis
- TCP/IP troubleshooting
- SSH authentication investigation
- Linux log analysis (`journalctl`)
- OpenSSH administration
- MITRE ATT&CK mapping
- SOC incident documentation
