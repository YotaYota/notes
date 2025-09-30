# YubiKey

[yubikey-manager](https://github.com/Yubico/yubikey-manager/)

```sh
sudo apt install yubikey-manager
```

## Commands

Connected YubiKeys

```sh
ykman list --serials
```

```sh
ykman --device <serial number> info
```

- `ykman info`
- `ykman fido info`
- `ykman fido access change-pin`
- `ykman fido credentials list`

