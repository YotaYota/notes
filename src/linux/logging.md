# Log

**syslog.service** sends all mesages to system logger and puts it in `/var/log/syslog`, except auth messages.

**systemd-journald.service** listens for all messages on the system by way of the *syslog protocol*.

`journalctl` aggregates all the different sources into one journal.

rsyslog reads all systemd-journald messages and puts them in their respective log files.

`logrotate` is the tool for log management.

## Files

Logs go into */var/log/*.

- */var/log/syslog* contains all logs except auth
- */var/log/auth.log* auth messages
- */var/log/dmesg* kernel ring buffer messages
- */var/log/kern.log* superset of dmesg

## rsyslog

### Install

Install

```bash
sudo apt install rsyslog
```

Enable and start

```bash
systemctl enable rsyslog
systemctl start rsyslog
systemctl status rsyslog --no-pager
```

Check syntax

```bash
sudo rsyslogd -N1
```

Test

```bash
logger -t tut01 "hello from rsyslog tutorial 01"
sudo tail -n 50 /var/log/syslog  2>/dev/null || sudo tail -n 50 /var/log/messages
```

### Configuration

Use **RainerScript** format.

`/etc/rsyslog.d/10-first.conf`:

```
# Write only messages tagged "tut02" to a custom file
if ($programname == "tut02") then {
    action(type="omfile" file="/var/log/rsyslog/myfirst.log")
    # no 'stop' here: allow normal distro handling to continue
}
```

On Ubuntu, create the directory and set permissions

```bash
sudo mkdir -p /var/log/rsyslog
sudo chown syslog:adm /var/log/rsyslog
sudo chmod 0755 /var/log/rsyslog
```

Restart

```bash
sudo systemctl restart rsyslog
systemctl status rsyslog --no-pager
```

Test and verify

```bash
logger -t tut02 "hello from rsyslog tutorial 02"
tail -f /var/log/rsyslog/myfirst.log
```


Look in */etc/rsyslog.conf*. It might contain old syntax like `$FileCreateMode 0640` and modern RainerScript syntax like `module(load="imuxsock")`, both are valid - but RainerScript is preferred. Leave */etc/rsyslog.conf* as it is. Do not try to "modernize" the legacy lines — rsyslog understands them.

Add your own rules under ``/etc/rsyslog.d/*.conf`` in RainerScript syntax.

- `imjournal` reads from systemd's journal (Usually in Debian-based systems). Feeds messages to rsyslog.
- `imuxsock` reads from the traditional syslog socket.
- `imtcp`recieve logs over TCP.

### Log Pipeline

Logs --> [Input] --> [Ruleset] --> [Actions/Outputs]

- Inputs – how logs arrive. Examples: `imuxsock` (syslog socket), `imjournal` (systemd journal), `imfile` (text files).
- Rulesets – the logic in between. They hold filters and actions and decide what happens to each message.
- Actions – where logs go. Actions execute serially in the order they appear. Examples: files (`omfile`), remote syslog (`omfwd`), or modern targets like Kafka (`omkafka`).
    - Earlier actions can modify or discard messages before later ones run.
    - Include snippets in */etc/rsyslog.d/* are processed in lexical order (e.g., *10-first.conf* runs before *50-extra.conf*).
    - Concurrency can be introduced by giving an action its own queue (*action.queue.\**) or by using separate rulesets/workers.

**Note**: rsyslog executes rules sequentially. The order of actions and included files can change results. Rules in the same file run top to bottom. Files in /etc/rsyslog.d/ are processed in lexical order (e.g., 10-first.conf runs before 50-extra.conf). An earlier rule can discard or modify messages, so later rules may never see them. Eg `stop` prevents further processing of a message.


### Configure rsyslog server

*/etc/rsyslog.d/10-receiver.conf*:

```
# Load UDP input
module(load="imudp")

# A ruleset just for messages received via this UDP listener
ruleset(name="rs-from-udp") {
    action(type="omfile" file="/var/log/rsyslog/remote.log")
    # This ruleset is used only for the UDP input below.
    # Local system logs continue to use the default distro config.
}

# Assign the UDP input to the ruleset above
input(type="imudp" port="514" ruleset="rs-from-udp")
```

The restart rsyslog

```bash
sudo systemctl restart rsyslog
systemctl status rsyslog -l --no-pager
rsyslogd -N1
```

---

Old format:

*/etc/rsyslog.conf* and */etc/rsyslog.d/<file name>.conf* has the format of

```
# Facility.priority		 location 
mail.err                 /var/mail/log.err

# wildcards
kern.*                   /var/log/kern.log
*lpr.*                   /var/log/lpr.log
```

### Forwarding

**gelf**.

---

## logrotate

Runs through SystemD `logrotate.service` and `logrotate.timer` that runs daily by default.

*/etc/logrotate.conf* is the main configuration file. It can include other files, such as */etc/logrotate.d/*.

```bash
logrotate --version
```

Verify it works

```bash
logrotate --debug /etc/logrotate.conf
```

settings under /etc/logrotate.d/ ovveride the main settings in /etc/logrotate.conf for specific log files.

Example:

```
/var/log/rsyslog/*/*.log {
  rotate 1
  daily
  sharedscripts
  missingok
  notifempty
  delaycompress
  maxsize 50M
  compress
  postrotate
    /usr/lib/rsyslog/rsyslog-rotate
  endscript
}
```


Logrotate is often configured system-wide to run once per day via **/etc/cron.daily/logrotate**.

