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

```
ssh-keygen -t rsa
```

creates
_~/.ssh/id_rsa_ and _~/.ssh/id_rsa.pub_
which are cryptographically linked together.

_Note_: A convenient way to copy you id to server

```
ssh-copy-id user@ip
```

## Server

Set _.ssh/_ folder permissions to 700.
Set _~/.ssh/authorized_keys_ permissions to 600.

Add the client's public key (_id_rsa.pub_) into _authorized_keys_

### Turn of password login

Open _/etc/ssh/sshd_config_

Set

- `PasswordAuthentication no`
- `ChallengeResponseAuthentication no`
- `UsePAM no`

Restart ssh daemon

```
systemctl restart sshd
```
