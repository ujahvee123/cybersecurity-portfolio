# Proactive Threat Hunting Report

## Hunt Title
Credential-Guessing Activity Against LINUX01 SSH Service

## Organization
TechCrush Financial Services

## Hunt Objective
Determine whether multiple external sources are conducting credential-guessing attacks against LINUX01, and whether their behavior differs in a way that indicates different levels of risk.

## Hunting Hypothesis
Multiple external sources may be conducting credential-guessing attacks against LINUX01, and their behavior may differ enough to distinguish targeted attacks from automated/opportunistic scanning.

## Scope
### Systems
LINUX01

### Time Period
August 31, 08:15 - 10:03 (simulated)

### Data Sources
Linux SSH authentication log (evidence/auth.log)

## Methodology
1. Reviewed the full authentication log to identify all source IPs and login outcomes.
2. Investigated each external source IP individually using grep.
3. Counted failed attempts per source using grep, sort, and uniq.
4. Compared which usernames each source targeted, and whether those usernames were valid accounts or flagged as invalid.
5. Built a full timeline including both legitimate and suspicious events.
6. Assessed each source separately against the hypothesis.

## Search Commands
grep "203.0.113.50" evidence/auth.log
grep "198.51.100.77" evidence/auth.log
grep "Failed password" evidence/auth.log | grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort | uniq -c

## Indicators Identified

| Indicator Type | Value |
|---|---|
| IP Address | 203.0.113.50 |
| IP Address | 198.51.100.77 |
| Target Account (valid, compromised) | alice |
| Target Account (valid, attempted) | root |
| Target Accounts (invalid) | admin, backup, test, guest |

## Timeline
- evidence/hunt-timeline.txt - full event timeline

## Findings

### Finding HUNT-001: Successful login following targeted credential guessing (203.0.113.50)
**Evidence:** 4 failed attempts against admin, root, and alice (twice), followed by 1 successful login as alice, all within 47 seconds.
**Risk:** Medium-High - the attacker guessed a valid, existing account name and succeeded, distinguishing this from opportunistic scanning.
**Recommendation:** Escalate immediately; validate the alice login with the account owner; review post-login activity.

### Finding HUNT-002: Automated scanning against nonexistent accounts (198.51.100.77)
**Evidence:** 3 failed attempts, all against usernames flagged "invalid user" (backup, test, guest) - none of these accounts exist on LINUX01. No successful authentication occurred.
**Risk:** Low - consistent with automated internet-wide scanning rather than targeted attack; no evidence of success or valid-account knowledge.
**Recommendation:** Log and monitor; no immediate escalation required unless this source is seen elsewhere or its behavior changes.

## Hypothesis Result
Partially Supported - both sources exhibited credential-guessing behavior as hypothesized, but with materially different risk profiles. 203.0.113.50 succeeded against a real account and requires escalation. 198.51.100.77 showed only scanning behavior against nonexistent accounts and does not warrant the same urgency.

## Recommended Actions
1. Escalate the 203.0.113.50 / alice login sequence to incident response for validation.
2. Log 198.51.100.77 for continued monitoring without immediate escalation.
3. Implement the detection logic in detections/ssh-suspicious-authentication-rule.md to flag similar patterns automatically.

## Detection Improvement Opportunities
See detections/ssh-suspicious-authentication-rule.md for proposed detection logic distinguishing targeted credential attacks from generic scanning noise.

## Analyst Conclusion
The threat hunt identified two distinct external sources conducting SSH credential-guessing activity, with meaningfully different risk levels based on account validity and authentication success rather than attempt volume alone. The hypothesis was partially supported, and the resulting analysis produced a concrete detection improvement opportunity in addition to escalation recommendations.

## Files / Evidence Included
- evidence/auth.log - raw SSH authentication log
- evidence/hunt-timeline.txt - full event timeline
- notes/hypothesis.md - hunting hypothesis
- detections/ssh-suspicious-authentication-rule.md - proposed detection logic
