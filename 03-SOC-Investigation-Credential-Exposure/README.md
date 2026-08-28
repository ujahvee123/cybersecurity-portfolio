# SOC Investigation 01: Insecure Credential Transmission Detection & Response

## Objective
Detect, analyze, and respond to a security weakness involving user credentials being transmitted without encryption, and demonstrate a corresponding firewall-based control as part of the response process.

## Scenario
During routine network traffic review, a packet capture was analyzed for a web application login flow to determine whether authentication data was transmitted securely. Following detection of a weakness, firewall administration was performed to demonstrate the ability to implement network-layer controls as part of a broader hardening response.

## Tools Used
- Wireshark (packet capture analysis)
- Windows PowerShell (firewall administration)
- Windows Defender Firewall

## Skills Demonstrated
- Packet capture analysis and HTTP protocol inspection
- Display filtering to isolate relevant traffic
- Identification of cleartext credential exposure
- Firewall rule administration (read, create, verify, remove)
- Security control recommendation and hardening reasoning

## Environment
- Local Windows machine, PowerShell run with Administrator privileges
- Wireshark analyzing a provided .pcap capture of traffic to `demo.testfire.net` (a publicly available, intentionally vulnerable demo banking application used for security testing practice)

## Investigation / Methodology

**Phase 1 — Detection**
1. Loaded the provided .pcap file into Wireshark.
2. Applied the display filter `http` to isolate HTTP traffic.
3. Identified a `POST /doLogin HTTP/1.1` request in the traffic.
4. Expanded the "HTML Form URL Encoded" section to inspect submitted form data.

**Phase 2 — Analysis**
5. Confirmed the request was sent over plain HTTP (port 80), not HTTPS (port 443) — no TLS encryption applied.
6. Extracted the submitted form fields directly from the packet in plaintext.

**Phase 3 — Response / Hardening Demonstration**
7. Verified Windows Defender Firewall was active across all profiles using `Get-NetFirewallProfile`.
8. Reviewed existing inbound allow rules using `Get-NetFirewallRule`.
9. Created a test inbound block rule (`TEST-Block-Port-8081`) to demonstrate the ability to restrict traffic on a specific port.
10. Verified the rule was active and correctly configured.
11. Removed the test rule and confirmed clean removal.

## Findings
- The login form at `demo.testfire.net/doLogin` transmitted credentials over unencrypted HTTP.
- Extracted form fields: `uid` (username) = `Cybersecurity`, `passw` (password) = `This is my password`.
- Because no TLS encryption was in use, this data was fully readable to anyone capturing traffic on the same network path.
- The local Windows Firewall was confirmed active and correctly enforcing rules across all profiles, and was demonstrated to be fully administrable.

## Analysis
The core issue is a **protocol-level weakness**, not a firewall misconfiguration — HTTP does not encrypt data in transit, so any credentials submitted through it are exposed regardless of firewall configuration. This distinguishes two security layers: **transport encryption** (missing here) and **network access control** (firewall, correctly configured). A well-configured firewall does not compensate for the absence of TLS — both layers are needed.

## Response / Remediation
- **Immediate:** Flag the affected login endpoint for migration to HTTPS/TLS.
- **Demonstrated control:** Firewall rule administration performed to show that unnecessary or risky inbound services can be identified and blocked at the host level as a compensating control.
- **Longer-term:** Enforce HTTPS-only policy (e.g., HSTS) across all authentication endpoints.

## Lessons Learned
- Firewalls and encryption solve different problems — a secure firewall posture does not mean credentials are safe if the application layer is misconfigured.
- Capturing literal field names (`uid`, `passw`) rather than paraphrasing preserves investigative accuracy.

## Recommendations
1. Migrate the login endpoint to HTTPS immediately; treat as high priority.
2. Periodically audit web applications for HTTP-only authentication forms.
3. Maintain firewall rule hygiene; rules were safely tested and removed without residual clutter.

## MITRE ATT&CK Mapping
- **T1040 – Network Sniffing:** The technique an attacker would use to intercept the cleartext credentials demonstrated in this capture.

## Conclusion
This investigation identified a real, demonstrable credential exposure risk caused by unencrypted HTTP transmission, paired with a hands-on demonstration of host-based firewall administration. Together, they show the ability to detect a security weakness through traffic analysis and take direct, verifiable action on system-level controls — two core Tier 1 SOC analyst competencies.

## Files / Evidence Included
- Wireshark: HTTP filter view showing GET/POST request sequence
- Wireshark: Expanded POST packet showing extracted form field values
- PowerShell: Firewall profile status
- PowerShell: Inbound rule listing
- PowerShell: Test rule creation and confirmation
- PowerShell: Test rule removal and confirmation
