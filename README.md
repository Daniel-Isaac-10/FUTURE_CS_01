# FUTURE_CS_01
A Future Interns Internship Program 

# FUTURE_CS_01 — Task 1: Web Application Security Testing (OWASP Juice Shop)

**Intern:** Daniel Isaac E  
**Project:** Future Interns — Cyber Security Internship  
**Date:** 2025-11-10

---

## One-line summary
Performed a hands-on web application security test on a local OWASP Juice Shop instance. Demonstrated a stored input issue in the product review feature (stored payload accepted by server). Findings, evidence and remediation recommendations included.

---

## What I did
- Ran OWASP Juice Shop locally and assessed the product review flow.  
- Used Burp Suite (Proxy + Repeater) to capture and manipulate HTTP traffic.  
- Submitted a crafted review payload and confirmed the server accepted it (201 Created).  
- Captured UI screenshots and HTTP request/response evidence.  
- Wrote a short, actionable report with impact and remediation guidance.

---

## Tools used
- OWASP Juice Shop (Docker image)  
- Burp Suite (Community) — Proxy, HTTP history, Repeater  
- Chromium / Firefox (proxied)  
- Docker & basic shell tools for evidence collection  
- Markdown → exported to PDF for the final report

---

## Lab: quick reproduction (local, safe)
> These commands assume you have Docker installed and are running locally. This is a controlled lab — do not run against external/production targets.

1. Start Juice Shop:
```bash
docker run --rm -d -p 3000:3000 bkimminich/juice-shop
Configure your browser to use Burp Suite as an HTTP(S) proxy (default: 127.0.0.1:8080). Import Burp CA cert if you want HTTPS interception.

In Burp:

Ensure Proxy → Options has a listener on 127.0.0.1:8080.

Keep Intercept OFF while browsing; use HTTP history and Repeater for edits.

In browser, open the product page:

bash
Copy code
http://localhost:3000/#/product/5
Capture the review submission request in Burp, send it to Repeater, and replace the request body with the test payload:

json
Copy code
{
  "message": "<script>alert('XSS-POC')</script>",
  "author": "you@example.com"
}
Then Send in Repeater.

Observe the server response (201 Created / {"status":"success"}) and refresh the product page to confirm the review is persisted.

Evidence included
All captured evidence — screenshots of the UI, HTTP request/response captures and tool logs — is available in the evidence/ folder of this repository. Reviewers can find the visual PoC and the recorded HTTP exchanges there.

Impact & recommendation (short)
Issue class: Stored input persistence (Stored XSS risk, CWE-79).

Risk: If stored HTML/JS is ever rendered without encoding, attackers could execute scripts in users’ browsers (session theft, account compromise, UI manipulation, phishing).

Fixes (priority):

Server-side sanitization of user-supplied content (whitelist if HTML is allowed).

Context-sensitive output encoding on every render.

Enforce CSP and secure cookie flags (HttpOnly, Secure, SameSite).

Add automated checks/tests to ensure consistent encoding across views.

What this deliverable contains
A concise report describing the finding, impact and remediation advice.

Screenshots showing the PoC in the web UI.

Captured HTTP request/response evidence from Burp Suite.
All artifacts are bundled in the repository under the evidence/ and reports/ folders.

Next steps (recommended)
Apply sanitization + encoding fixes and re-test the same flow.

Audit all rendering paths (admin UI, email templates, API consumers, mobile clients).

Add a short automated test that ensures injected markup is not executed in any client.

Notes
This work was performed against a local lab (OWASP Juice Shop) and is intended as a learning exercise and portfolio deliverable. If you have any questions or want me to expand this into an executive slide or a short video walkthrough, I’ll make it in a minute.

Thanks — Daniel.
