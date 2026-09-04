# DHCP Investigation (DORA Process)

## Objective

Investigate how a client dynamically obtains an IP address using the DHCP protocol and analyze the complete DORA exchange in Wireshark.

## Lab Setup

| Device | Role | IP |
|---------|------|----|
| Kali Linux | Packet Analyzer | 10.0.2.3 |
| Ubuntu | DHCP Client | 10.0.2.15 |
| VirtualBox NAT | DHCP Server | 10.0.2.2 |

---

## Tools Used

- Ubuntu 24.04
- Kali Linux
- Wireshark
- `dhclient`

---

## Commands

Release the current lease:

```bash
sudo dhclient -v -r enp0s3
```

Request a new IP address:

```bash
sudo dhclient -v enp0s3
```

---

## Wireshark Filter

```text
dhcp
```

---

## DORA Process

### 1. DHCP Discover

The client broadcasts a request searching for any available DHCP server.

**Screenshot:** `screenshots/02-dhcp-dora.png`

### 2. DHCP Offer

The DHCP server offers an available IP address (10.0.2.7).

### 3. DHCP Request

The client requests the offered IP address from the server.

### 4. DHCP ACK

The DHCP server confirms the lease and assigns the IP address.

---

## Evidence

### DHCP Client Output

Shows the complete lease renewal process using `dhclient`.

**Screenshot:** `screenshots/dhclient-terminal.png'

### Wireshark Capture

Captured the full DORA sequence:

- DHCP Release
- DHCP Discover
- DHCP Offer
- DHCP Request
- DHCP ACK

**Screenshot:** `screenshots/dhcp-dora.png'

---

## Security Findings

- DHCP uses UDP ports **67** (server) and **68** (client).
- Discover and Request are broadcast because the client initially has no valid IP.
- A rogue DHCP server could respond faster and assign malicious network settings, enabling traffic interception.

---

## Skills Demonstrated

- DHCP analysis
- DORA sequence identification
- Wireshark packet investigation
- Linux networking with `dhclient`
