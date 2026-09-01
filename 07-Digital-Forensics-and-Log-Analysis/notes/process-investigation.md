# Process and Network Investigation

## Commands Used
ps aux | head
ss -tun

## Objective
The purpose of this investigation was to identify running processes
and active network connections on the system, as part of building
familiarity with what "normal" system activity looks like; a
necessary baseline before suspicious activity can be recognized.

## Observations

### Processes
1. PID 1, USER root, COMMAND /sbin/init — core system init process
2. PID 65, USER root, COMMAND /usr/lib/systemd/systemd-journald — system logging service
3. PID 226, USER message+, COMMAND @dbus-daemon --system — system message bus

### Network Connections
No active TCP/UDP connections were observed at the time of this check
(`ss -tun` returned no results). This is expected behavior in this
WSL environment because it is when no active network sessions are open.

## Analyst Questions
- Is each process expected on this system?
- Which user owns each process?
- Is each network connection expected, or does it need investigation?
- Does any remote address require further checking?
