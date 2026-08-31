# Incident Response Report

## Incident Title
Suspicious SSH Authentication Activity

## Organization
TechCrush Financial Services

## Affected Asset
LINUX01

## Incident Status
Under Investigation

## Initial Detection
Multiple failed SSH authentication attempts were detected from
203.0.113.50 followed by a successful authentication event.

## Analyst
Victor

## Observed Facts
- Multiple SSH authentication failures occurred.
- The source IP was 203.0.113.50.
- The accounts targeted were admin, root, and alice.
- A successful login for alice occurred shortly after the failures.

## Analysis
The authentication sequence is suspicious because multiple failed
authentication attempts against several accounts were followed by a
successful login to the alice account from the same source IP address.
This pattern may indicate password guessing, credential compromise,
or another unauthorized authentication attempt.
Further investigation is required before confirming compromise.

## Recommended Containment Actions
1. Validate whether alice was expected to authenticate from the source IP.
2. Review active and recent sessions associated with the account.
3. Consider temporarily disabling or restricting the account if
unauthorized access is confirmed.
4. Block the source IP according to organizational policy if it is
determined to be malicious.

## Potential Eradication Actions
If unauthorized access is confirmed:
- Reset affected credentials.
- Review the account for unauthorized changes.
- Search for persistence mechanisms.
- Review system processes and services.
- Investigate the initial access method.

## Recovery Actions
- Restore authorized account access.
- Verify authentication controls.
- Monitor future authentication events.
- Confirm that no unauthorized persistence remains.
- Document lessons learned.
