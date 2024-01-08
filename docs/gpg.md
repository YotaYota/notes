# GnuPG

List key ids
```
gpg -k
```

Export full key
```
gpg --armor --export <KEY ID>
```

Reload card
```
gpg --card-status
```

## Encrypt

```
gpg --symmetric -o encrypted.gpg unencrypted.txt
```

## Decrypt

```
gpg --decrypt encrypted.gpg
```
