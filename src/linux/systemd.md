# SystemD

SystemD is an init-service.

**Units** are things that systemd can handle for you. A **service** is one kind of unit.

## Unit File

A simple structure

```
[Unit]
Description=The name of the service
Wants=other services to start up
Requires=requires named service to start or fail
After=guaranteed to start after named service

[Service]
Type=simple
User=root
Group=root
Environment=SOMEVAR=someval
WorkingDirectory=/root
ExecStart=/root/my_program.sh
TimeoutSec=30s
Restart=always
RestartSec=15s

[Install]
WantedBy=multi-user.target
```

Unit files are stored in (by priority):

1. `/etc/systemd/system/` 
2. `/run/systemd/system/` runtime
3. `/lib/systemd/system/` installed service files (might be overriden by packet upgrades)

`systemctl list-unit-files`

Make local edits in `/etc`. `systemctl edit name.service` will create a file in `/etc/systemd/system/` where you can override partial settings from those in `/lib/systemd/system/`.

## `systemctl`

- `systemctl status name.service`
- `systemctl start name.service`
- `systemctl stop name.service`
- `systemctl restart name.service`
- `systemctl enable name.service` starts when system boots
- `systemctl disable name.service` does not start when system boots
- `systemctl daemon-reload`

**Note**: `.service` can be omitted in `name.service`.

## `journalctl`

```sh
journalctl -fu <service name> --since "2025.01.13 12:00:00"
```

where `-f` is short for `--follow`.
