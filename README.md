# Cybersecurity Fundamentals: A Comprehensive Guide

**From Foundations to Authorized Penetration Testing**

**Author:** Tito Oscar Mwaisengela  
**GitHub:** https://github.com/tito-devsec/  
**Role:** Computer Science Student (UDSM)  
**Specialization:** Cybersecurity • Authorized Penetration Testing • Secure Software  
**Level:** Beginner → Intermediate

---

## Introduction

Cybersecurity is about protecting systems, data, and people by understanding how attacks occur and how to prevent them.

This guide is designed to:
- Build strong cybersecurity fundamentals
- Help beginners understand security concepts clearly
- Help intermediate learners connect theory to practical attacks
- Prepare readers for authorized penetration testing, CTFs, and real-world security work

> ⚠️ All techniques discussed here are for educational and authorized testing only. Never test systems without written permission.

---

## 1. The CIA Triad — Core Security Principles

The CIA Triad is the foundation of cybersecurity decisions. Every attack and defense maps back to one or more of these principles.

| Principle | Meaning | Real-World Example | Tools / Commands |
| --- | --- | --- | --- |
| Confidentiality | Only authorized users can access data | Encrypting stored passwords | `openssl`, `gpg` |
| Integrity | Data cannot be altered without detection | Detecting tampered files | `sha256sum`, `gpg --verify` |
| Availability | Systems are accessible when needed | Protection against DDoS | `ping`, `hping3` |

Why this matters:
- SQL Injection → breaks Confidentiality & Integrity
- Malware → breaks Integrity & Availability
- DDoS → breaks Availability

Practical integrity check:
```bash
# Create a file
echo "sensitive_data" > secret.txt

# Generate hash
sha256sum secret.txt > hash.txt

# Simulate tampering
echo "tampered_data" > secret.txt

# Detect change
sha256sum secret.txt | diff - hash.txt
```
If hashes differ → integrity is compromised.

---

## 2. How the Web Works (And How Attackers Exploit It)

Understanding web architecture is mandatory for web security.

Web request flow:
```
Browser → DNS → Web Server → Application → Database → Response
```

Typical steps:
1. Browser resolves domain via DNS (`nslookup`)
2. Browser sends HTTP/HTTPS request
3. Web server (Apache/Nginx) receives it
4. Application logic (PHP, Python, Node) processes the request
5. Database query executes
6. Response is returned

Common attack points:

| Layer | Attacks | Tools |
| --- | --- | --- |
| DNS | Subdomain takeover | `sublist3r` |
| Web Server | Misconfiguration | `nikto` |
| Application | SQLi, XSS | Burp Suite, OWASP ZAP |
| Database | Data exfiltration | `sqlmap` |

---

## 3. HTTP vs HTTPS — Why Encryption Matters

| Protocol | Port | Security | Risk |
| --- | ---: | --- | --- |
| HTTP | 80 | Plaintext | Credentials/sniffing |
| HTTPS | 443 | TLS-encrypted | Safer when properly configured |

TLS inspection (authorized):
```bash
openssl s_client -connect target.com:443
nmap --script ssl-enum-ciphers -p 443 target.com
sslscan target.com
```
Weak ciphers or misconfigured TLS increase risk of MITM attacks.

---

## 4. Core Internet Protocols & Attacker Perspective

| Protocol | Port | Purpose | Example Attack |
| --- | ---: | --- | --- |
| TCP | — | Reliable transport | SYN flood |
| DNS | 53 | Name resolution | DNS spoofing |
| HTTP | 80 | Web | XSS, SQLi |
| HTTPS | 443 | Secure web | TLS downgrade |
| SSH | 22 | Remote access | Brute force |
| SMB | 445 | File sharing | EternalBlue-style exploits |

Example (authorized SYN flood test):
```bash
hping3 --syn -p 80 --flood target.com
```
Only perform such tests with explicit authorization.

---

## 5. OWASP Top 10 (2021) — Explained

A selection of key issues and examples:

- A01 – Broken Access Control  
  Problem: Unauthorized access to resources.  
  Quick check:
  ```bash
  curl https://target.com/admin
  ```
  If accessible → potential high severity issue.

- A02 – Cryptographic Failures  
  Problem: Sensitive data not protected in transit or at rest.

- A03 – Injection (SQLi, Command Injection)  
  Problem: Unsanitized input allows remote code/data access.
  Example:
  ```bash
  sqlmap -u "https://target.com/login.php" --forms --dbs
  ```

- A05 – Security Misconfiguration  
  Problem: Default or exposed settings.
  ```bash
  dirb https://target.com
  nmap --script http-enum target.com
  ```

- A07 – Authentication Failures  
  Problem: Weak credential handling, session issues.
  ```bash
  hydra -L users.txt -P rockyou.txt target.com http-post-form "/login:..."
  ```

- A10 – SSRF  
  Problem: Server-side requests to attacker-controlled URLs.
  Example:
  ```bash
  curl "https://target.com/api?url=http://169.254.169.254"
  ```
  This can expose cloud instance metadata.

---

## 6. Authorized Penetration Test Workflow (High-Level)

Example (basic automation for authorized, scoped tests):
```bash
#!/bin/bash

echo "[+] Recon"
nmap -sC -sV target.com
sublist3r -d target.com

echo "[+] Enumeration"
dirsearch -u https://target.com
nikto -h https://target.com

echo "[+] Vulnerability Scan"
nuclei -u https://target.com

echo "[+] Exploit Testing"
sqlmap -u "https://target.com/search?q=*" --dbs
```
Always stay within scope and use written permission.

---

## 7. Defense & Hardening (Blue Team Mindset)

Security posture requires defensive controls and monitoring:
```bash
ufw enable
apt install fail2ban clamav
echo "server_tokens off;" >> /etc/nginx/nginx.conf
```
Defense is essential — detection, patching, and least privilege policies reduce risk.

---

## 8. Impact Summary (Example Mapping)

| Attack | CVSS | Business Impact | Detection |
| --- | ---: | --- | --- |
| SQLi | 9.8 | Data breach | Database logs, alerts |
| XSS | 6.1 | Session hijack | CSP, monitoring |
| RCE | 9.8 | Full takeover | HIDS, EDR |
| SSRF | 8.2 | Cloud compromise | Proxy logs, anomaly detection |

---

## Learning Outcomes

After studying this guide you should be able to:
- Understand how common attacks work
- Identify why vulnerabilities exist
- Conduct authorized penetration testing (ethically and within scope)
- Produce clear CTF and lab reports
- Build a professional cybersecurity portfolio

---

## Ethical Notice

> ⚠️ All content is for educational and authorized security testing only. Unauthorized access is illegal and unethical.

---

## Author

**Tito Oscar Mwaisengela**  
Junior → Intermediate Cybersecurity Practitioner  
**GitHub:** https://github.com/tito-devsec/

---

If you want, I can now:
1. Create these files in the repository `tito-devsec/CyberSecurty-fundamentals` (please confirm spelling and that the repo exists), or  
2. Provide these files as a ZIP you can upload, or  
3. Provide the exact git commands to create the repo locally and push.

Which option do you prefer?
