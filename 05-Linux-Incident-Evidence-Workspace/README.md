# Linux Incident Evidence Workspace & Network Isolation

## Objective
Establish a secure, isolated Linux workspace for handling evidence from a suspected network breach, apply appropriate file permissions to protect sensitive evidence from tampering, and configure host-based firewall rules to block a known-malicious source while preserving legitimate access.

## Scenario
Acting as the primary responder for a suspected network breach at TechCrush Financial Services, I needed to quickly prepare an isolated workspace to handle evidence, restrict access to that evidence, and block network communication with the attacker's identified command server.

## Tools Used
- Bash (Ubuntu terminal)
- UFW (Uncomplicated Firewall)

## Methodology
1. Confirmed current location using `pwd`, then created a top-level evidence workspace `quarantine_zone` in the home directory.
2. Created three subdirectories to separate evidence types: `pcap_logs`, `malware_samples`, `threat_intel`.
3. Created an empty file `suspect_ips.txt` inside `threat_intel`, then copied it into `pcap_logs` to confirm the workspace structure functioned correctly.
4. Restricted permissions on `suspect_ips.txt` to `640` (owner: read/write, group: read-only, others: no access), reflecting that this file contains sensitive threat data.
5. Checked existing firewall state with `sudo ufw status` **before making changes**, to establish a baseline.
6. Added a `DENY` rule against the attacker's identified IP (`198.51.100.22`).
7. Added an `ALLOW` rule for port 443, so the team could continue securely pulling threat intelligence feeds.
8. Enabled the firewall and confirmed the final rule set.

## Findings

### Finding LNX-001: FTP and SMTP ports open to all sources
**Evidence:** `sudo ufw status` (baseline, before any changes) shows `ALLOW` rules on port 21 (FTP) and port 25/tcp (SMTP), both scoped to `Anywhere`, in addition to port 22 (SSH).
**Risk:** Medium: FTP transmits authentication credentials in cleartext, the same category of exposure identified in the earlier Wireshark HTTP credential investigation. SMTP open to all sources increases exposure to spam-relay abuse if misconfigured. Both are reachable from any internet-facing source rather than a restricted, trusted range.
**Recommendation:** Confirm whether FTP and SMTP services are actually required on this host. If not required, remove the `ALLOW` rules for ports 21 and 25. If required, restrict the source to known, trusted IP ranges instead of `Anywhere`.

### Finding LNX-002: Malicious source IP successfully blocked
**Evidence:** `sudo ufw deny from 198.51.100.22` was executed and confirmed active in the resulting `sudo ufw status` output, showing a `DENY` rule against that address.
**Risk:** Informational: this reflects a control correctly applied, not a vulnerability.
**Recommendation:** No action needed. Monitor for repeat attempts from related or adjacent IP ranges.

### Finding LNX-003: Evidence file access restricted appropriately
**Evidence:** `ls -l suspect_ips.txt` confirmed permissions changed from `644` (rw-r--r--) to `640` (rw-r-----) after running `chmod 640 suspect_ips.txt`.
**Risk:** Informational: permissions now correctly limit write access to the owner only, and remove all access for users outside the owning group, protecting evidence integrity.
**Recommendation:** For group-level access control, apply `chgrp soc_analysts quarantine_zone` so only authorized SOC team members inherit group-level read access to the workspace.

## Analysis
The evidence workspace was created and permissioned correctly, and the firewall response to the identified threat (blocking the attacker's IP while preserving port 443 access) was executed in the correct order — baseline check, then deny, then allow, then enable — which avoided any risk of losing intended access mid-change. Separately, the pre-existing firewall baseline revealed two ports (21, 25) open to all sources that were not part of this incident but represent a standing exposure worth addressing independently of the current investigation.

## Recommendations
1. Investigate whether FTP (21) and SMTP (25) are required services on this host; restrict or remove if not.
2. Apply group ownership (`chgrp soc_analysts quarantine_zone`) to formally restrict the evidence workspace to authorized personnel.
3. Continue monitoring for further connection attempts from the blocked IP or related ranges.

## Conclusion
This exercise demonstrated the ability to rapidly construct a secure evidence-handling workspace under time pressure, apply least-privilege file permissions to protect sensitive investigative data, and make firewall changes in a safe, verified sequence that blocks a known threat without disrupting legitimate access — while also surfacing an unrelated but real finding (open FTP/SMTP) through routine baseline review.

## Files / Evidence Included
- Directory structure creation (mkdir, touch, cp, ls)
- Permission change on evidence file (chmod 640)
- Firewall baseline, deny rule, allow rule, and enable confirmation (ufw)
