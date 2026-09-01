# Incident Response Fundamentals: Suspicious SSH Authentication Activity

## Objective
Simulate the early stages of an incident response process — detection,
evidence collection, timeline construction, and initial reporting —
for a suspicious SSH authentication event, following the standard
Incident Response lifecycle.

## Scenario
TechCrush Financial Services detected multiple failed SSH authentication
attempts against a Linux host (LINUX01), followed by a successful login.
As the responding analyst, I built an evidence workspace, documented the
observed authentication events, constructed a timeline, and produced an
initial incident report with recommended containment, eradication, and
recovery actions.

## Tools Used
- Bash (Ubuntu terminal)
- Markdown (for structured reporting)

## Skills Demonstrated
Incident response lifecycle documentation, evidence handling and
organization, timeline construction, authentication log interpretation,
structured incident reporting.

## Methodology
1. Created a structured incident response workspace (`evidence`, `reports`,
   `timelines`, `notes`, `screenshots`).
2. Documented raw SSH authentication log evidence, including three failed
   login attempts followed by one successful login, all from the same
   source IP.
3. Built a timeline table correlating each authentication event with
   its time, source, and target account.
4. Produced a full incident report covering initial detection, observed
   facts, analysis, and recommended containment, eradication, and
   recovery actions.

## Findings

### Finding IR-001: Repeated authentication failures preceding a successful login
**Evidence:** `evidence/ssh-authentication-events.txt` shows three failed
login attempts (against `admin`, `root`, and `alice`) from `203.0.113.50`
within a 20-second window, followed by a successful login to `alice`
from the same IP 28 seconds later.
**Risk:** Medium: this pattern is consistent with password-guessing
activity. It cannot yet be confirmed as unauthorized without validating
whether `alice` was expected to authenticate from this source.
**Recommendation:** Validate the successful login with the account
owner and review recent session activity before deciding on further
containment action.

## Analysis
The sequence of failed attempts against multiple accounts, followed
immediately by a successful login to one of those same accounts from
the identical source IP, is a recognized indicator of brute-force or
credential-guessing activity. This is a fact-based observation, not
a confirmed compromise, further validation (contacting the account
owner, reviewing session activity) is required before escalating.

## Recommended Containment Actions
1. Validate whether `alice` was expected to authenticate from `203.0.113.50`.
2. Review active and recent sessions associated with the account.
3. Consider temporarily restricting the account if unauthorized access
   is confirmed.
4. Block the source IP per organizational policy if confirmed malicious.

## Conclusion
This exercise demonstrated the ability to organize an incident response
workspace, document raw evidence accurately, build a supporting timeline,
and produce a structured report that distinguishes confirmed facts from
analysis requiring further validation, a foundational Tier 1 SOC
analyst skill.

## Files / Evidence Included
- `evidence/ssh-authentication-events.txt` — raw authentication log evidence
- `timelines/ssh-incident-timeline.md` — event timeline
- `reports/incident-report.md` — full incident report
