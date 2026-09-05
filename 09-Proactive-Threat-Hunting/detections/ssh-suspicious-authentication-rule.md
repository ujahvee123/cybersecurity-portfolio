# Detection Idea: Suspicious SSH Authentication Pattern

## Objective
Identify repeated failed SSH authentication attempts targeting a valid
account, followed by successful authentication from the same source.

## Logic
IF:
- Multiple failed SSH logins occur
- From the same source IP
- Within a short time window
- At least one attempt targets a valid (existing) account
- Followed by successful authentication on that account

THEN:
Generate a medium or high priority alert for analyst review.

## Note on Tuning
Failed attempts against nonexistent ("invalid user") accounts alone,
with no subsequent success, are common internet-wide scanning noise
therefore should not trigger the same priority as this rule.

## Analyst Actions
1. Validate the account owner's activity.
2. Review source IP history across other systems.
3. Review post-authentication activity.
4. Escalate according to SOC procedures if compromise indicators exist.
