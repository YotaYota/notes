# systemd

- Name: `name.service`

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

Unit files are stored in
- `/etc/systemd/system/`

`systemctl list-unit-files`

## `systemctl`

- `systemctl start name.service`
- `systemctl stop name.service`
- `systemctl enable name.service` starts when system boots
- `systemctl disable name.service` does not start when system boots

Status

```sh
systemctl restart name.service
```

```sh
systemctl status name.service
```

**Note**: `.service` can be omitted in `name.service`.

## Logs

```sh
journalctl -fu <service name>
```

where `-f` is short for `--follow`.
