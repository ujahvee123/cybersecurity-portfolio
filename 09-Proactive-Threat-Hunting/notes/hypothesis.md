## Hypothesis
Multiple external sources may be conducting credential-guessing attacks
against LINUX01, and their behavior may differ enough to distinguish
targeted attacks from automated/opportunistic scanning.

## Expected Evidence
- Repeated failed logins from external sources
- Some sources targeting only nonexistent usernames (scanning behavior)
- Some sources achieving success on a real account (higher-risk targeting)

## Data Source
Linux SSH authentication log (evidence/auth.log)
