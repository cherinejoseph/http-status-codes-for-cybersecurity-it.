# 🛡️ HTTP Status Codes for Cybersecurity Professionals

A focused guide for cybersecurity analysts, pentesters, SOC teams, and ITSec engineers. This cheat sheet highlights the most security-relevant HTTP status codes, including **attack implications**, **misconfiguration clues**, and **remediation strategies**.


## 🛡️ Security Considerations

HTTP status codes can reveal important information during penetration testing, incident response, or system hardening:

- **401 Unauthorized** – Ensure sensitive endpoints are protected with proper auth mechanisms (e.g., token-based, MFA).
- **403 Forbidden** – Indicates access is blocked; verify role-based access controls (RBAC) are implemented correctly.
- **404 Not Found** – Used to obscure the existence of sensitive files (e.g., `/admin`, `/login`, `/wp-admin`).
- **429 Too Many Requests** – Can be used to detect scraping or brute-force attempts. Implement rate limiting and IP blacklisting.
- **5xx Errors** – Might suggest unstable or vulnerable backend systems. Investigate root causes and monitor logs.

🔐 **Pro Tip**: Never expose stack traces or server details (like Apache or PHP versions) in 5xx responses—this can leak exploitable data.


---

## 🔐 4xx – Client Errors (Security-Focused)

| Code | Meaning           | Security Use Case                                                                 | Resolution / Next Step                                                  |
|------|-------------------|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| 400  | Bad Request       | Often triggered by malformed payloads during fuzzing or XSS/SQL injection attempts. | 🔍 Review request structure and input validation. Harden parsing logic. |
| 401  | Unauthorized      | Indicates protected resource; common during brute-force and auth bypass attempts. | 🔐 Ensure proper authentication methods (e.g., tokens, MFA). Log attempts. |
| 403  | Forbidden         | Confirms resource exists but access is denied. Can indicate privilege enforcement. | 🛠️ Validate RBAC/ABAC policies. Monitor for forced browsing attempts.   |
| 404  | Not Found         | Helps obscure sensitive endpoints. Attackers may use scanning to detect admin panels. | 🔎 Implement honeypots or false paths. Hide real endpoints where possible. |
| 405  | Method Not Allowed | Occurs when HTTP verb (e.g., POST, DELETE) is disallowed. Useful for probing REST APIs. | 🔐 Enforce strict HTTP method rules and return minimal error details.   |
| 408  | Request Timeout   | May indicate slow HTTP attacks (e.g., Slowloris).                                 | ⏱️ Monitor for abnormal request delays. Tune web server timeout configs. |
| 409  | Conflict          | Could reveal resource logic or race condition bugs. Rare but exploitable in logic flaws. | 🔒 Ensure atomic operations and validate state on critical actions.     |
| 429  | Too Many Requests | Indicates rate limiting. Attackers may hit this during brute-force or scraping.    | 🧱 Enable WAF/Rate limiting. Alert on repeated offenders (block IPs).    |

---

## 🔥 5xx – Server Errors (Attack Surface Clues)

| Code | Meaning               | Security Use Case                                                       | Resolution / Next Step                                                |
|------|-----------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------|
| 500  | Internal Server Error | May leak server logic or stack traces. Attackers use it to test for input validation issues. | 🚨 Disable stack trace output. Harden input validation and log errors securely. |
| 502  | Bad Gateway           | Can occur during SSRF or upstream service probing.                      | 🔎 Check for SSRF attempts. Use firewall rules and validate URLs internally. |
| 503  | Service Unavailable   | Often targeted in DoS/DDoS attacks or by monitoring service uptime.     | 📉 Implement auto-scaling, load balancing, and alerting for downtime. |
| 504  | Gateway Timeout       | Could suggest vulnerable or slow backend services. Used in fuzzing attacks. | ⏱️ Optimize timeouts and monitor backend latency. Investigate slow responses. |

---

## 🟢 2xx – Success (Recon Indicators)

| Code | Meaning | Security Use Case                                                 | Action / Monitoring Step                                         |
|------|---------|-------------------------------------------------------------------|------------------------------------------------------------------|
| 200  | OK      | Confirms the existence and accessibility of a resource. Useful in enumeration. | 📡 Monitor for unusual GET/POST patterns or spikes in endpoint access. |
| 201  | Created | May indicate successful resource creation via API abuse.          | 🧪 Log and rate-limit resource creation endpoints. Validate inputs. |
| 204  | No Content | Can be exploited for stealthy actions (e.g., silent deletes or pings). | 📋 Log all request types regardless of content in the response.   |

---

## 🟡 3xx – Redirection (Security Bypass, Phishing, Enumeration)

| Code | Meaning           | Security Use Case                                                           | Resolution / Next Step                                             |
|------|-------------------|-----------------------------------------------------------------------------|--------------------------------------------------------------------|
| 301  | Moved Permanently | May reveal internal URLs or legacy services via redirection.                | 🛑 Avoid leaking internal paths. Use relative paths and sanitize headers. |
| 302  | Found             | Can be exploited for open redirect attacks or phishing.                     | 🚧 Validate redirect URLs. Avoid user-controlled redirect targets. |
| 304  | Not Modified      | Reveals caching behavior; may be leveraged in timing or cache poisoning attacks. | 🧹 Sanitize and validate caching headers (ETag, If-Modified-Since). |

---

## 🚨 Common Security Threats & HTTP Code Indicators

| Threat Type              | Status Code Indicators | Detection / Mitigation Tips                                       |
|--------------------------|------------------------|--------------------------------------------------------------------|
| Brute-force attacks      | 401, 429               | Log and block IPs. Use CAPTCHA/MFA.                               |
| Directory/File Enumeration | 403, 404, 200         | Deploy honeypots, monitor access to sensitive paths.              |
| SSRF                     | 502, 504               | Validate and whitelist internal URLs. Monitor unexpected patterns.|
| Input fuzzing            | 400, 500               | Sanitize all input, use structured exception handling.            |
| Open redirects/phishing  | 302                    | Disallow external URL redirection unless absolutely needed.       |
| DoS/Slowloris            | 408, 503               | Use timeouts, reverse proxies, and rate limits.                   |

---

## 🧰 Useful for:

- 🔍 **Penetration Testers**: Response codes help determine endpoint behavior and access control.
- 🛡️ **SOC Analysts**: Monitor anomalies in 4xx/5xx patterns.
- 🔒 **Blue Team/Defenders**: Implement detections based on suspicious HTTP response codes.
- ⚙️ **DevSecOps Engineers**: Harden error handling and monitor metrics in CI/CD pipelines.

---

## 📚 References

- [OWASP HTTP Security Response Codes](https://owasp.org/www-community/Improper_Error_Handling)
- [MDN Web Docs – HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- [RFC 6585 – HTTP Rate Limiting (429)](https://datatracker.ietf.org/doc/html/rfc6585)

---

## 🧩 Want to Contribute?

Feel free to fork, add case studies, or link this to your detection rules for SIEM/WAF/EDR pipelines. PRs welcome!

