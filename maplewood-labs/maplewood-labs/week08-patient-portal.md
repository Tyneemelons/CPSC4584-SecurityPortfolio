 Patient Portal Login Page Behaving Strangely
Course: CPSC 4584 | Special Topics in Information Security Date: October 19, 2026 Analyst: Tyree Anderson Case ID: WEB-2026-1019-001

Incident Summary
Three patients reported unexpected behavior while using the Maplewood patient portal login page. SOC investigation and HTTP response analysis identified four security vulnerabilities involving credential exposure in URLs, missing cookie security attributes, missing security headers, and unsafe redirect handling.

HTTP Vulnerability Findings
Finding	Location	Observed Value	Vulnerability	Severity
1	Login request method	GET /login?username=jsmith&password=Summer2026!	Credential Exposure in URL	High
2	Set-Cookie header	Set-Cookie: sessionid=a3f8c2d1; Path=/	Missing HttpOnly and Secure Attributes	High
3	Response headers	No Strict-Transport-Security, No Content-Security-Policy, No X-Frame-Options	Missing Security Headers	Medium
4	Redirect handling	next=https://external-site.com/collect accepted and used as an external redirect target	Unvalidated Redirect	High
Analysis Summary
Finding 1: The login request used the GET method and included credentials directly in the URL query string. This creates a risk that usernames and passwords may be stored in browser history, application logs, proxy logs, analytics platforms, screenshots, or copied links. The recommended remediation is to submit credentials in the request body using POST over HTTPS.

Finding 2: The session cookie was issued without the HttpOnly and Secure attributes. Without HttpOnly, client-side scripts may be able to access the session token. Without Secure, the browser may send the cookie over unsecured HTTP connections. The recommended remediation is to add HttpOnly and Secure attributes and review additional protections such as SameSite.

Finding 3: The application response did not include Strict-Transport-Security, Content-Security-Policy, or X-Frame-Options headers. Missing security headers reduce browser-enforced protections against clickjacking, content injection, and protocol downgrade attacks. The recommended remediation is to implement and test appropriate security header configurations.

Finding 4: The application accepted an external destination through the next parameter and later redirected users to that location. This behavior creates phishing and social-engineering risks because users may be redirected to attacker-controlled websites. The recommended remediation is to validate redirect destinations against an approved allowlist or restrict redirects to trusted internal locations.

HTTP Header Inspection Commands Used
Command	Purpose
curl -I https://example.com	Retrieved HTTP response headers and identified the Content-Type as text/html; charset=utf-8. No Set-Cookie header was observed.
curl -I + grep security headers	Filtered for common security-related headers. No matching headers were returned during testing.
curl -v + grep request/response	Displayed request lines beginning with ">" and response lines beginning with "<" to examine the HTTP exchange between client and server.
curl -IL + grep HTTP/Location	Examined redirect behavior and returned an HTTP/2 200 response without a Location header.
Escalation Summary
The investigation confirmed four security findings within the Maplewood patient portal login process. Credentials were transmitted through a URL using a GET request, session cookies lacked HttpOnly and Secure attributes, important security headers were absent from HTTP responses, and user-controlled redirect destinations were accepted without proper validation. These weaknesses increase the risk of credential exposure, session hijacking, browser-based attacks, and phishing activity. Recommended next steps include migrating authentication to POST requests over HTTPS, implementing secure cookie attributes, configuring appropriate security headers, and validating redirect destinations before redirection. Tier 1 analyst responsibilities end with documenting evidence, assessing impact, and escalating findings. Application development, security engineering, remediation, testing, and deployment decisions belong to authorized personnel.

CPSC 4584 | Governors State University | Fall 2026
