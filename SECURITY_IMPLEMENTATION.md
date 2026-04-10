# SECURITY_IMPLEMENTATION.md

## 🔐 Introduction

This project focuses on identifying and fixing common web application vulnerabilities in a Flask-based IoT dashboard. The main vulnerabilities addressed are:

* Insecure Direct Object Reference (IDOR)
* Cross-Site Request Forgery (CSRF)

---

## 🚨 IDOR Vulnerability

### 📌 Description

IDOR (Insecure Direct Object Reference) occurs when an application allows users to access resources directly using user-controlled input (such as URL parameters) without proper authorization checks.

In this application, users could access different device dashboards by modifying the URL parameters (`username` and `session`).

---

### ❌ Before Fix

Users could access:

http://192.168.113.140:5000/device1/admin/abc123
http://192.168.113.140:5000/device1/hacker/xyz

➡️ No validation was performed
➡️ Any user could access another user's dashboard

---

### ✅ After Fix

Authorization check was implemented:

```python
if username not in logged_in or logged_in[username]['object'].session_id != session:
    abort(403)
```

---

### 🛡️ Result

* Unauthorized users receive **403 Forbidden**
* Only authenticated users can access their own dashboard
* IDOR vulnerability successfully prevented

---

### 📸 Screenshot (After Fix)

👉 Insert screenshot showing:

* URL `/device1/admin/abc123`
* 403 Forbidden response

---

## 🔒 CSRF Vulnerability

### 📌 Description

CSRF (Cross-Site Request Forgery) allows attackers to trick authenticated users into performing unwanted actions without their consent.

Initially, the application forms did not include CSRF protection, making them vulnerable.

---

### ❌ Before Fix

Forms were submitted without CSRF protection:

```html
<form method="POST" action="/login">
```

➡️ Requests could be forged from external sources

---

### ✅ After Fix

CSRF protection was implemented using Flask-WTF.

Added in backend:

```python
from flask_wtf.csrf import CSRFProtect

app.config['SECRET_KEY'] = 'mysecretkey123'
csrf = CSRFProtect(app)
```

Added CSRF token in forms:

```html
<input type="hidden" name="csrf_token" value="{{ csrf_token() }}">
```

---

### 🛡️ Result

* Each form includes a unique CSRF token
* Server validates the token before processing requests
* Unauthorized or forged requests are blocked

---

### 📸 Screenshot (CSRF Token)

👉 Insert screenshot showing:


* Hidden CSRF token in browser inspect (Elements tab)

---

## 🧪 Testing & Verification

* Attempted unauthorized access via URL manipulation → ❌ Blocked (403)
* Verified authorized user access → ✅ Allowed
* Tested form submission with CSRF token → ✅ Successful
* Verified CSRF token present in form → ✅ Confirmed

---

## ✅ Conclusion

The application has been successfully secured against:

* IDOR attacks through proper authorization checks
* CSRF attacks using token-based validation

These improvements ensure that only authorized users can access resources and all requests are verified for authenticity.

---

## 👨‍💻 Student Contribution

* Implemented Flask application setup
* Identified and fixed IDOR vulnerability
* Implemented CSRF protection using Flask-WTF
* Performed testing and validation
* Documented security implementation

---
