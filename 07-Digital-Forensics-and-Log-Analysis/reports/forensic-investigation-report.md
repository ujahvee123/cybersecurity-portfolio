# Digital Forensic Investigation: Suspicious SSH Authentication and Post-Authentication Activity

## Objective
Perform a forensic investigation correlating authentication, process, and network evidence to determine whether a suspicious SSH login on LINUX01 was followed by unauthorized or malicious activity, and to demonstrate proper evidence preservation and integrity verification throughout the process.

## Scenario
This investigation continues directly from the Day 1 incident response exercise. Following multiple failed SSH login attempts and one successful login to the alice account on LINUX01, additional evidence was collected showing a process execution and an outbound network connection occurring shortly afterward. As the investigating analyst, I preserved this evidence with integrity hashes, examined it individually, then correlated it into a single timeline to assess whether the activity represents a genuine security incident.

## Tools Used
- Bash (Ubuntu/WSL terminal)
- sha256sum (evidence integrity hashing)
- file, stat (file type and metadata inspection)
- ps aux, ss -tun (process and network baseline investigation)

## Skills Demonstrated
Digital forensics fundamentals, evidence preservation and integrity verification, disk forensics (file-type inspection), memory/process investigation, log correlation, IOC identification, timeline construction, forensic reporting.

## Methodology
1. Preserved the original SSH authentication evidence and generated a SHA-256 hash to establish a verifiable integrity baseline.
2. Investigated a sample file with a disguised extension (invoice.pdf) using file and stat, confirming that file extensions cannot be trusted as proof of file type.
3. Established a system baseline by reviewing running processes (ps aux) and active network connections (ss -tun).
4. Created process and network evidence files documenting the suspicious activity that followed the successful login.
5. Re-generated SHA-256 hashes across all evidence files to preserve integrity of the full evidence set.
6. Documented potential Indicators of Compromise (IOCs), explicitly noting that indicators require further investigation before being classified as confirmed malicious.
7. Built a correlated timeline joining authentication, process, and network evidence into a single sequence.
8. Assessed each piece of evidence individually, then assessed the correlated sequence as a whole.

## Findings

### Finding FOR-001: Suspicious process executed from /tmp following authentication
**Evidence:** evidence/process-events.txt shows a Python process (python3 /tmp/update-check.py) executed by user alice at 10:02:15, 14 seconds after her successful SSH login.
**Risk:** Medium: the script's filename suggests a routine update check, but this cannot be verified from the evidence collected. The /tmp directory is commonly used to stage attacker tools because it is world-writable and files are often cleared automatically, which makes execution from this location worth treating with suspicion regardless of the filename.
**Recommendation:** Preserve a copy of /tmp/update-check.py before it can be deleted or overwritten, and inspect its actual contents (e.g., using file and cat) rather than trusting its name.

### Finding FOR-002: Outbound connection to external IP following process execution
**Evidence:** evidence/network-events.txt shows an outbound TCP connection from 10.10.20.15 to 203.0.113.50 on port 443 at 10:02:45, 30 seconds after the suspicious process started.
**Risk:** Medium: port 443 traffic is encrypted, but encryption does not indicate the destination is trustworthy; attackers commonly use port 443 specifically because it blends in with normal outbound web traffic and is rarely blocked by default firewall rules.
**Recommendation:** Check 203.0.113.50 against threat intelligence sources (e.g., VirusTotal) to determine if it has a known bad reputation, and review firewall/proxy logs for any other connections to this address from other hosts.

### Finding FOR-003: Correlated event sequence increases overall suspicion
**Evidence:** timelines/correlated-incident-timeline.md shows failed logins, a successful login, process execution, and an outbound connection all occurring within a 91-second window, from a single source.
**Risk:** Medium-High: no single event in isolation proves compromise, but the tight timing and logical sequence (access, execution, external communication) matches a common pattern seen in real intrusions, and significantly raises the overall risk compared to any one finding alone.
**Recommendation:** Escalate to the incident response team for further investigation rather than closing this as a false positive; do not dismiss based on any single reassuring detail (friendly filename, encrypted port) in isolation.

## Evidence Integrity
SHA-256 hashes were generated for all evidence files (evidence/evidence-hashes.txt) to allow verification that evidence was not altered after collection.

## Potential Indicators of Compromise
- IP address: 203.0.113.50
- File/process: python3 /tmp/update-check.py
- Authentication pattern: multiple failed logins followed by a successful login from the same source

## Analysis
Individually, each piece of evidence has an innocent possible explanation — the process could genuinely be an update checker, and the network connection could be legitimate encrypted traffic. However, the correlation across three independent evidence sources (authentication, process, network), tightly clustered in time and following a logical attacker progression, substantially increases the overall risk assessment beyond what any single finding would justify alone. This is the core value of log correlation: individually ambiguous events can become a coherent, actionable pattern when examined together.

## Recommended Actions
1. Validate the alice authentication activity with the account owner.
2. Preserve and inspect /tmp/update-check.py before it can be modified or deleted.
3. Check the destination IP against threat intelligence sources.
4. Review other hosts for connections to the same external IP.
5. Escalate to the incident response team pending further investigation.
6. Consider containment measures per organizational incident response procedures if malicious activity is confirmed.

## Conclusion
This investigation demonstrated the ability to preserve evidence with cryptographic integrity checks, avoid trusting superficial indicators (filenames, "secure" ports) without verification, and correlate multiple independent evidence sources into a single, well-reasoned timeline. The available evidence supports escalation for further investigation; it does not, on its own, prove confirmed compromise, a distinction the report makes explicit rather than overstating.

## Files / Evidence Included
- evidence/suspicious-auth.log — SSH authentication log
- evidence/process-events.txt — process execution evidence
- evidence/network-events.txt — network connection evidence
- evidence/evidence-hashes.txt — SHA-256 integrity hashes for all evidence
- notes/file-analysis.md — disguised file-type investigation
- notes/process-investigation.md — system baseline documentation
- notes/potential-iocs.md — indicators of compromise
- timelines/correlated-incident-timeline.md — correlated event timeline
