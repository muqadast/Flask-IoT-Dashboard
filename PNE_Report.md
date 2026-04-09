# Protection Needs Elicitation (PNE) Report
## IoT Dashboard — CYC386 DevSecOps Sprint, Spring 2026

---

## 1. System Overview
The IoT Dashboard is a Flask web application that allows authenticated
users to monitor and control their IoT devices (thermostats, cameras,
motion sensors) through a browser-based interface.

---

## 2. Asset Identification

| Asset | Description | Sensitivity |
|-------|-------------|-------------|
| User credentials | Username + hashed passwords | Critical |
| Session tokens | Flask session cookies | Critical |
| Device control commands | On/off/config sent to devices | High |
| Sensor readings | Temperature, motion, camera data | High |
| Admin panel access | Full system access | Critical |
| Device ownership data | Which user owns which device | High |
| API keys | Device authentication tokens | High |

---

## 3. Threat Actors

| Actor | Motivation | Capability |
|-------|-----------|------------|
| Unauthenticated attacker | Access devices without login | Low-Medium |
| Authenticated malicious user | Access other users' devices (IDOR) | Medium |
| External web attacker | CSRF — forge device control requests | Medium |
| Clickjacking attacker | Embed dashboard, steal admin clicks | Low |
| Insider threat | Abuse admin access | High |

---

## 4. Protection Needs

| ID | Protection Need | Priority | Vulnerability Addressed |
|----|----------------|----------|------------------------|
| PN-01 | Only the device owner may view, edit, or control their device | Critical | IDOR |
| PN-02 | All state-changing forms must include anti-forgery tokens | Critical | CSRF |
| PN-03 | Dashboard must not be embeddable in external iframes | High | Clickjacking |
| PN-04 | Session cookies must be Secure, HttpOnly, and SameSite=Strict | Critical | Session hijacking |
| PN-05 | Device name inputs must be sanitized before storage and display | High | XSS |
| PN-06 | Login must enforce rate limiting and lockout after 5 failures | High | Brute force |
| PN-07 | All dependencies must be free of known CVEs | Medium | Supply chain |

---

## 5. Mapping to OWASP Top 10

| Protection Need | OWASP Category |
|----------------|----------------|
| PN-01 IDOR | A01:2021 Broken Access Control |
| PN-02 CSRF | A01:2021 Broken Access Control |
| PN-03 Clickjacking | A05:2021 Security Misconfiguration |
| PN-04 Session | A07:2021 Identification & Authentication Failures |
| PN-05 XSS | A03:2021 Injection |
| PN-07 Dependencies | A06:2021 Vulnerable Components |