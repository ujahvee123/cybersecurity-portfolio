# Correlated Incident Timeline

| Time | Evidence Source | Event |
|---|---|---|
| 10:01:14 | Authentication Log | Failed login for admin |
| 10:01:20 | Authentication Log | Failed login for root |
| 10:01:33 | Authentication Log | Failed login for alice |
| 10:02:01 | Authentication Log | Successful login for alice |
| 10:02:15 | Process Evidence | python3 executed /tmp/update-check.py |
| 10:02:45 | Network Evidence | Outbound connection to 203.0.113.50:443 |

## Initial Analysis
The evidence shows multiple failed authentication attempts followed
by a successful login. Shortly after authentication, a process was
executed from the /tmp directory and an outbound network connection
was established.

The sequence is suspicious and should be investigated further.
