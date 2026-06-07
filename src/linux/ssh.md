# SSH

| Packet                      |
| --------------------------- |
| packet length               |
| padding amount              |
| payload                     |
| padding                     |
| message authentication code |

_Note_: Compression can be applied to the 'padding amount', 'payload' and 'padding' as well.

## Client

```bash
ssh-keygen -t rsa -b 4096 -C "comment or email"
```

creates *~/.ssh/id_rsa* and *~/.ssh/id_rsa.pub* which are cryptographically linked together.
The private key needs to be `rw` for user only, while the public key should be `r` for everyone.
the `~./ssh/` dir should be `rwx` for user only.

**Note**: prefer `-t ed25519` over `-t rsa` if supported by the server.

**Note**: Use passkey

*~/.ssh/config*:

```conf
Host alias
  HostName ip
  User user
  IdentityFile ~/.ssh/id_rsa
```

**Note**: A convenient way to copy you id to server

```
ssh-copy-id -i path/to/key.pub user@ip
```

The keys needs to be added to the key-agent with `ssh-add`.

`eval "$(ssh-agent -s)"` to start the ssh agent.

`ssh-add ~/.ssh/id_rsa` to add the key to the agent.

## Server

```bash
systemctl status sshd
```

```bash
journalctl -fu sshd
```

Set *.ssh/* folder permissions to 700.
Set *~/.ssh/authorized_keys* permissions to 600.

Add the client's public key (*id_rsa.pub*) into *authorized_keys*.

Host keys are found in _/etc/ssh/_ and are used to identify the server to the client. They are generated with `ssh-keygen -A` and should be kept secret.

*/etc/ssh/sshd_config* is the main configuration file for the ssh server.

```
# Prefer having a non-root user with sudo privileges and disable root login
PermitRootLogin yes
# Disable password authentication to force the use of ssh keys
PasswordAuthentication no

KbdInteractiveAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```

```bash
systemctl restart sshd
```
