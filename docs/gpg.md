# GnuPG

GPG (PGP) follows a design of a smart card. This includes 3 sets of keys.

List public key ids
```
gpg -k
```

Export full key
```
gpg --armor --export <KEY ID>
```

Shows GPG keys
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
