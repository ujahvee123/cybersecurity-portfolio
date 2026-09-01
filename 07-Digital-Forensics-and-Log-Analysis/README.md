# Digital Forensics and Log Analysis: Suspicious SSH Authentication and Post-Authentication Activity

## Objective
Perform a forensic investigation correlating authentication, process, and network evidence to determine whether a suspicious SSH login on LINUX01 was followed by unauthorized or malicious activity, and to demonstrate proper evidence preservation and integrity verification throughout the process.

## Scenario
This investigation continues directly from the 06-Incident-Response-Fundamentals exercise. Following multiple failed SSH login attempts and one successful login to the alice account on LINUX01, additional evidence was collected showing a process execution and an outbound network connection occurring shortly afterward.

## Tools Used
- Bash (Ubuntu/WSL terminal)
- sha256sum (evidence integrity hashing)
- file, stat (file type and metadata inspection)
- ps aux, ss -tun (process and network baseline investigation)

## Skills Demonstrated
Digital forensics fundamentals, evidence preservation and integrity verification, disk forensics (file-type inspection), memory/process investigation, log correlation, IOC identification, timeline construction, forensic reporting.

## Full Report
See [reports/forensic-investigation-report.md](./reports/forensic-investigation-report.md) for the complete investigation, including all findings, risk ratings, and recommendations.

## Key Findings (Summary)
- **FOR-001 (Medium):** Suspicious Python process executed from /tmp shortly after login — filename cannot be trusted as proof of purpose.
- **FOR-002 (Medium):** Outbound connection to an external IP over port 443 — encryption does not indicate the destination is trustworthy.
- **FOR-003 (Medium-High):** The correlated sequence across all three evidence sources is more suspicious than any single event alone.

## Files
- `evidence/` — raw evidence files and SHA-256 integrity hashes
- `notes/` — file-type analysis, process/network baseline, potential IOCs
- `timelines/` — correlated incident timeline
- `reports/` — full forensic investigation report
