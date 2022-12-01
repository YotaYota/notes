# Git

Verify connection

```
ssh -T git@github.com
```

Verity private key is generated and loaded into SSH

```
ssh-add -l -E sha256
```

Start SSH agent

```
eval "$(ssh-agent -s)"
```

Add key

```
ssh-add <id_ed25519 file>
```
