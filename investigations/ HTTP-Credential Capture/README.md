# HTTP Credential Capture

> A SOC investigation demonstrating how HTTP Basic Authentication exposes credentials over an unencrypted network.

---

## 🎯 Objective

Capture HTTP authentication traffic using Wireshark and demonstrate that credentials can be recovered because HTTP transmits data without encryption.

---

## 🧪 Lab Environment

| Component | Details |
|-----------|---------|
| **Attacker / Web Server** | Kali Linux |
| **Victim / Client** | Ubuntu 24.04 |
| **Server IP** | `10.0.2.3` |
| **Client IP** | `10.0.2.15` |
| **Network** | VirtualBox NAT Network (`10.0.2.0/24`) |
| **Protocol** | HTTP (TCP/80) |

---

## 📖 Scenario

An internal web application uses **HTTP Basic Authentication**. A user logs in from an Ubuntu workstation while a SOC analyst captures the network traffic in Wireshark to determine whether the credentials are exposed.

---

## 🛠️ Tools Used

- Kali Linux
- Ubuntu 24.04
- Apache2
- Wireshark
- curl

---

## Step 1 — Configure the Apache Server

A protected directory was created using HTTP Basic Authentication.

**Screenshot**

![Apache Running](screenshots/apache-running.png)

---

## Step 2 — Generate Authentication Traffic

The Ubuntu client authenticated to the web server using `curl`.

```bash
curl -u sevi:Sevi@123 http://10.0.2.3/secure/
```

**Expected Output**

```text
Welcome Sevi
```

**Screenshot**

![Ubuntu Login](screenshots/ubuntu-curl-login.png)

---

## Step 3 — Capture the HTTP Session

Wireshark captured the HTTP GET request using the display filter:

```text
http
```

The request was sent from **10.0.2.15** to **10.0.2.3** over **TCP Port 80**.

**Screenshot**

![HTTP GET Packet](screenshots/http-get.png)

---

## Step 4 — Analyze the Authorization Header

Expanding the HTTP protocol reveals the **Authorization: Basic** header.

Wireshark automatically decoded the Base64 value and displayed the credentials.

**Recovered Credentials**

- **Username:** `Sevi`
- **Password:** `Sevi@123`

**Screenshot**

![Decoded Credentials](screenshots/credentials-decoded.png)

---

## 📊 Wireshark Evidence

| Field | Value |
|--------|-------|
| Protocol | HTTP |
| Source IP | `10.0.2.15` |
| Destination IP | `10.0.2.3` |
| Destination Port | **80** |
| Authentication | HTTP Basic |
| Credentials | `Sevi : Sevi@123` |

---

## 🚨 Security Analysis

HTTP **does not encrypt** application-layer data. Basic Authentication only **Base64 encodes** the username and password, making them easily recoverable from packet captures.

**Risk:** High — Credential Exposure

---

## 🛡️ Detection Summary

| Indicator | Observation |
|-----------|-------------|
| Protocol | HTTP |
| Port | 80 |
| Detection Method | Wireshark Packet Analysis |
| IOC | `Authorization: Basic` header |
| Severity | **High** |

---

## ✅ Conclusion

This investigation successfully demonstrated that HTTP Basic Authentication exposes user credentials over the network. A SOC analyst can identify the Authorization header, recover the transmitted credentials, and classify the activity as a high-risk credential exposure due to the lack of encryption.
