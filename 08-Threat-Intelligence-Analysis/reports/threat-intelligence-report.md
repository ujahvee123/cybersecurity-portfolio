# Suspicious SSH Authentication Activity Involving External Source 203.0.113.50

## Project Overview
A beginner-level threat intelligence investigation performed in the TechCrush Financial Services simulated SOC environment, analyzing suspicious SSH authentication activity and applying threat intelligence principles to assess risk without overstating conclusions.

## Objective
Analyze suspicious SSH authentication activity on LINUX01 to determine whether a successful login was preceded by patterns consistent with credential-based attack behavior, and produce a risk-rated assessment to support the SOC's next investigative steps.

## Tools Used
- Bash (Ubuntu/WSL terminal)
- grep, sort, uniq

## Skills Demonstrated
Threat intelligence fundamentals, IOC identification, log analysis (grep/sort/uniq), risk assessment, cautious evidence-based reporting.

## Observed Activity
Log analysis (grep, sort, uniq) on evidence/authentication-events.txt identified the following from source IP 203.0.113.50:

 4 failed authentication attempts
 3 distinct accounts targeted: admin, root, alice
 1 successful authentication, to the alice account, occurring 13 seconds after the final failed attempt

## Evidence Collected
- evidence/authentication-events.txt — raw SSH authentication log
- evidence/iocs.txt — extracted indicators

## Indicators Identified
- IP Address: 203.0.113.50
- Target Host: LINUX01
- Target Account: admin
- Target Account: root
- Target Account: alice (successful)

## Assessment
The pattern: repeated failures across multiple accounts from a single source, immediately followed by success on one of the previously-failed accounts which is consistent with password-guessing behavior. This is a recognized credential-attack signature, not a random or unrelated sequence of events.

However, the evidence available at this stage is limited to authentication metadata alone. It does not include post-login activity, account-owner confirmation, or corroborating data from other systems. For that reason, this finding should be classified as suspicious, pending further investigation. Overstating this conclusion (e.g., "the account is compromised") would not be supportable by the evidence collected and could misdirect the response effort.

## Recommended Actions
1. Validate with alice (or her manager, if unavailable) whether she authenticated from this source at this time.
2. Search internal logs for other activity from 203.0.113.50 across additional hosts.
3. Review any activity performed by the alice account immediately following the successful login.
4. Check 203.0.113.50 against external threat intelligence sources for known-bad reputation.
5. Escalate to the incident response team if validation cannot confirm the login was authorized.

## Conclusion
This investigation identified a credential-attack pattern based on log evidence alone, and appropriately distinguished suspicion from confirmation. The recommended actions are structured to close that evidence gap efficiently, rather than assuming the worst-case interpretation without support.
