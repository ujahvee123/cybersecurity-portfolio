# SSH Authentication Timeline

| Time | Event | Source | Account |
|---|---|---|---|
| 10:01:14 | Failed login | 203.0.113.50 | admin |
| 10:01:20 | Failed login | 203.0.113.50 | root |
| 10:01:33 | Failed login | 203.0.113.50 | alice |
| 10:02:01 | Successful login | 203.0.113.50 | alice |

## Initial Assessment

Multiple failed authentication attempts were followed by a successful
authentication event from the same source IP address.

Further investigation is required to determine whether the successful
login was authorized.
