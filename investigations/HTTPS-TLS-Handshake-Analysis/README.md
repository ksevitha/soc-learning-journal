# HTTPS & TLS Handshake Analysis

> Demonstrating how HTTPS encrypts credentials and protects web traffic from network interception.

## 🎯 Objective

Build a secure HTTPS web server in Kali Linux, access it from an Ubuntu client, and verify with Wireshark that TLS encrypts the authentication traffic.

---

## 🧪 Lab Environment

| Component | Value |
|-----------|------|
| Attacker / Server | Kali Linux |
| Client | Ubuntu 24.04 |
| Server IP | 10.0.2.3 |
| Client IP | 10.0.2.15 |
| Protocol | HTTPS (TLS) |
| Port | 443 |
| Capture Tool | Wireshark |

---

## 📖 Scenario

A user accesses an internal website hosted on Kali Linux using HTTPS. The objective is to validate that credentials are no longer visible during packet capture after enabling TLS.

---

## 🛠️ Steps Performed

### 1. Enable SSL in Apache

Enabled the SSL module and HTTPS virtual host.

```bash
sudo a2enmod ssl
sudo a2ensite default-ssl
```

**Evidence**

![Enable SSL](screenshots/enable-ssl.png)

---

### 2. Configure & Restart Apache

Generated a self-signed certificate, configured `default-ssl.conf`, and restarted Apache successfully.

```bash
sudo systemctl restart apache2
sudo systemctl status apache2
```

**Evidence**

![Apache Running](screenshots/apache-running.png)

---

### 3. Generate HTTPS Traffic (Ubuntu)

From the Ubuntu client, access the secure website using curl.

```bash
curl -k -u sevi:Sevi@123 https://10.0.2.3/secure/
```

**Purpose:** Generate authenticated HTTPS traffic for Wireshark analysis.

### 4. Capture the TLS Handshake

Applied the Wireshark display filter:

```text
tls
```

Observed the **Client Hello** packet initiated by Ubuntu.

**Evidence**

![TLS Client Hello](screenshots/tls-client-hello.png)

---

### 5. Verify Encrypted Traffic

After the TLS handshake, HTTP data became encrypted as **TLS Application Data**.

No username or password was visible.

**Evidence**

![Encrypted Application Data](screenshots/encrypted-application-data.png)

---

## 🔍 Wireshark Findings

| Observation | Result |
|-------------|--------|
| Client Hello | Present |
| Server Hello | Present |
| TLS Handshake | Successful |
| Certificate Exchange | Successful |
| Application Data | Encrypted |
| Credentials Visible | ❌ No |

---

## 📊 Security Comparison

| Feature | HTTP | HTTPS |
|----------|------|-------|
| Port | 80 | 443 |
| Encryption | ❌ | ✅ TLS |
| Username Visible | Yes | No |
| Password Visible | Yes | No |
| Packet Content | Plaintext | Ciphertext |

---

## 🧠 Key Learning

- Enabled HTTPS using Apache SSL.
- Generated a self-signed TLS certificate.
- Captured the TLS handshake in Wireshark.
- Identified the **Client Hello** packet.
- Verified that authentication data becomes **encrypted application data**.
- Demonstrated why HTTPS prevents credential exposure during packet analysis.

---

## ✅ Conclusion

The investigation confirmed that TLS successfully protects user authentication. Unlike HTTP, HTTPS encrypts the payload before transmission, preventing attackers or network analysts from viewing usernames and passwords in Wireshark.
