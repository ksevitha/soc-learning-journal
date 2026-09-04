# HTTP Credential Capture

## Objective

Demonstrate how HTTP Basic Authentication exposes credentials in clear text and identify the evidence using Wireshark.

---

## Lab Environment

| Role | Machine | IP Address |
|------|---------|-----------|
| Web Server | Kali Linux | 10.0.2.3 |
| Client | Ubuntu | 10.0.2.15 |
| Network | VirtualBox NAT Network | 10.0.2.0/24 |

---

## Attack Scenario

A user accessed an internal web application using HTTP Basic Authentication.

Because the application used HTTP instead of HTTPS, the credentials were transmitted without encryption.

The objective was to determine whether a network analyst could recover the username and password from captured traffic.

---

## Tools Used

- Wireshark
- Apache2
- curl
- Ubuntu 24.04
- Kali Linux

---

## Methodology

### 1. Configure the HTTP Server

A protected directory was created on the Apache web server using Basic Authentication.

### 2. Generate Client Traffic

The Ubuntu client authenticated using curl:

```bash
curl -u sevi:Sevi@123 http://10.0.2.3/secure/
```

### 3. Capture Network Traffic

Wireshark captured the HTTP session on the Kali interface using the filter:

```text
http
```

### 4. Analyze the Packet

The HTTP GET request contained an Authorization header.

Wireshark automatically decoded the Base64 value and revealed:

- Username: **sevi**
- Password: **Sevi@123**

---

## Wireshark Evidence

| Evidence | Observation |
|----------|-------------|
| Protocol | HTTP |
| Source IP | 10.0.2.15 |
| Destination IP | 10.0.2.3 |
| Destination Port | 80 |
| Authentication | HTTP Basic |
| Credentials | Sevi : Sevi@123 |

---

## Why This Is a Security Risk

HTTP does not encrypt application-layer data.

Basic Authentication only encodes credentials using Base64, which is reversible and readable by anyone capable of capturing network traffic.

A SOC analyst can therefore recover usernames and passwords directly from packet captures.

---

## Detection Summary

- **Protocol:** HTTP
- **Port:** 80
- **Indicator:** Authorization: Basic
- **Risk:** Credential Exposure
- **Severity:** High

---

## Outcome

The investigation successfully demonstrated credential theft over HTTP and validated that unencrypted web authentication can be detected and analyzed using Wireshark.
