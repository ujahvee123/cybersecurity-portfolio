# Windows Endpoint Security Audit — TCFS-WIN-001

## Objective
Assess the security posture of a Windows workstation using built-in Windows tools and PowerShell, and document findings, evidence, and remediation recommendations from a Junior SOC Analyst perspective.

## Scenario
Acting as a Junior SOC Analyst for the fictional organization TechCrush Financial Services, I was asked to perform a baseline security assessment of an employee workstation prior to it being considered fully compliant with company security standards.

## Tools Used
- Windows PowerShell (standard and Administrator)
- Windows Event Viewer
- Microsoft Defender / Windows Security

## Skills Demonstrated
Endpoint inspection, PowerShell-based security auditing, Defender/firewall/update assessment, local account and privilege review, disk encryption assessment, Windows Event Log familiarity, evidence-based security reporting.

## Environment
- Host: `DESKTOP-REMEBU7` (documented as case-study host **TCFS-WIN-001**)
- User: `USER`
- OS: Windows 10 Pro, Version 2009, Build 22000

## Investigation / Methodology
1. Identified host and current user (`hostname`, `whoami`).
2. Collected OS version and build (`Get-ComputerInfo`).
3. Checked Microsoft Defender status (`Get-MpComputerStatus`).
4. Reviewed installed updates, sorted by most recent (`Get-HotFix`).
5. Reviewed local user accounts (`Get-LocalUser`) and Administrators group membership (`Get-LocalGroupMember`).
6. Attempted BitLocker status check as standard user (denied); re-ran as Administrator to confirm accurate results (`Get-BitLockerVolume`).
7. Confirmed Windows Security Event Log was active and accessible (Event Viewer).

Note: Windows Firewall status and rule administration were assessed as part of a related prior project — see [03-SOC-Investigation-Credential-Exposure](../03-SOC-Investigation-Credential-Exposure).

## Findings

| ID | Title | Evidence | Risk | Recommendation |
|---|---|---|---|---|
| WIN-001 | Installed updates are outdated | `Get-HotFix` shows most recent update dated 4/21/2026 — approximately 4 months before this assessment (8/28/2026) | Medium | Verify Windows Update is functioning correctly and install any pending security updates |
| WIN-002 | Disk encryption (BitLocker) is not enabled | `Get-BitLockerVolume` shows `VolumeStatus: FullyDecrypted`, `ProtectionStatus: Off`, `EncryptionPercentage: 0` | Medium–High (higher for portable devices) | Enable BitLocker on the OS volume, particularly if this is a portable device |
| WIN-003 | Defender real-time protection is active and current | `Get-MpComputerStatus` shows `AntivirusEnabled: True`, `RealTimeProtectionEnabled: True`, signatures updated same-day | Informational | No action needed — maintain current configuration |
| WIN-004 | Local administrator membership is minimal and expected | Only the built-in (disabled) Administrator account and the single working user account are members | Informational | No action needed — reflects least-privilege practice |
| WIN-005 | Security Event Logging is active | Security log actively recording (33,605+ events, User Account Management category visible) | Informational | Maintain logging; useful for future investigations |

## Analysis
The endpoint shows generally sound security hygiene — Defender is fully active, local privilege assignment is minimal and appropriate, and logging is functional. The two Medium-risk findings (patch currency and disk encryption) are both common, realistic gaps rather than critical failures, and both are independently addressable.

## Response / Remediation
- Verify Windows Update service is running correctly and check for pending updates manually via Settings → Windows Update.
- Enable BitLocker on the C: volume, particularly important if this device leaves a secured location.

## Lessons Learned
- An "Access Denied" error is not automatically a valid finding — it needed re-verification with elevated privileges before it could be trusted as evidence (BitLocker check).
- Not every observation is a problem: minimal admin membership and active Defender protection are findings too, just Informational ones — a professional report documents both what's working and what isn't.

## Recommendations
1. Investigate why Windows Update has not applied new patches in ~4 months.
2. Enable BitLocker disk encryption on the OS volume.
3. Continue current Defender configuration — no changes needed there.
4. Periodically re-run this baseline (e.g., quarterly) to catch drift.

## Conclusion
This audit demonstrated the ability to systematically assess a Windows endpoint's security posture using PowerShell and built-in Windows tools, distinguish confirmed evidence from assumptions, and produce risk-rated, evidence-backed findings and recommendations.

## Files / Evidence Included
- System identification (hostname, user, OS/build)
- Defender status output
- Installed update history
- Local users and administrator group membership
- BitLocker volume status (access-denied attempt + confirmed Administrator result)
- Security Event Log screenshot
