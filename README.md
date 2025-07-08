# Hack-Yourself-First

# 🔍 Vulnerability Assessment Report: Hack Yourself First

## 🖥️ Target Site
[https://hack-yourself-first.com](https://hack-yourself-first.com)

## 🛠 Tools Used
- Burp Suite Pro
- Repeater
- Intruder
- Manual testing
---
## 🧭 Recon & Setup
1. Open Burp Suite Pro → Go to **Dashboard > New Scan**
2. Enter the target URL: `https://hack-yourself-first.com`
3. Check **Target > Issues** tab for auto-detected vulnerabilities

📸 *The following screenshot shows the scanning of the website:*

![Burp Dashboard](website%20vulnerability_001.jpg)
---
## 🚨 Vulnerabilities Found

### 1. 🐞 SQL Injection (High Severity)

**Location 1**:  
`/CarsByCylinders?Cylinders=V12' OR '1'='1 --`  
➡️ Sends injected SQL — Response shows extra data  
✅ **Confirmed SQL Injection**

![SQLi Test](website%20vulnerability_002.jpg)

**Steps:**
- Sent request to **Repeater**
- Modified `Cylinders` parameter with `' OR '1'='1 --`
- Got valid response → more data returned

**Automated via Intruder:**
- Go to **Intruder > Positions**
- Set Attack Type: **Sniper**
- Load SQLi payloads
- Analyze response lengths for anomalies

![Intruder Setup](website%20vulnerability_003.jpg)
![Intruder Payloads](website%20vulnerability_004.jpg)

**Location 2**:  
`/Make/2?orderby=supercarid'`  
➡️ Tested with `orderby=1` — no error shown  
✅ **ORDER BY SQL Injection confirmed**
![SQLi Location 2](website%20vulnerability_005.jpg)
---
### 2. ✴️ Reflected XSS (High Severity)

**Parameter**: `searchTerm`  
Payloads tried:
- `searchTerm=<script>alert(1)</script>` ❌ Not executed
- `searchTerm=" onmouseover="alert(1)` ✅ **Triggered**
📸 *Payload and HTML response:*
![XSS Injection](website%20vulnerability_006.jpg)
✅ **Reflected XSS confirmed** — input reflected inside HTML attribute
![XSS Rendered Output](website%20vulnerability_007.jpg)
---
### 3. 💾 Stored XSS (High Severity)

**Location**: `/Supercar/10` → Add a comment and vote

**Payload:**
```html
<script>window.location="http://bootlesshacker.com";</script>

![Description of screenshot](website%20vulnerability_007.jpg)
![Description of screenshot](website%20vulnerability_008.jpg)
![Description of screenshot](website%20vulnerability_009.jpg)
![Description of screenshot](website%20vulnerability_010.jpg)
Confirmed Stored XSS via comment injection and redirection

** Key Takeaways**
SQL Injection and XSS vulnerabilities are still common in demo web apps
Manual + Burp Suite combo is powerful for finding input flaws
Always validate server-side and sanitize output to avoid XSS
All images are stored in this repo under the /images/ folder (or root if not moved)

## 🛑 Disclaimer
This vulnerability assessment was performed on [https://hack-yourself-first.com](https://hack-yourself-first.com), which is an intentionally vulnerable web application created for ethical hacking training.
This report is for **educational purposes only** and complies with all relevant ethical hacking guidelines.  
Please do not use any techniques mentioned here on unauthorized or live production systems.
