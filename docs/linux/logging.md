# Log

systemd-journald.service listens for all messages on the system by way of the *syslog protocol*.

`journalctl` aggregates all the different sources into one journal.

rsyslog reads all systemd-journald messages and puts them in their respective log files.

`logrotate` is the tool for log management.

## rsyslog

*/etc/rsyslog.conf* and */etc/rsyslog.d/<file name>.conf* has the format of

```
# Facility.priority		 location 
mail.err                 /var/mail/log.err

# wildcards
kern.*                   /var/log/kern.log
*lpr.*                   /var/log/lpr.log
```

## Files

Logs go into */var/log/*.

- */var/log/syslog* contains all logs except auth
- */var/log/auth.log* auth messages
- */var/log/dmesg* kernel ring buffer messages
- */var/log/kern.log* superset of dmesg
