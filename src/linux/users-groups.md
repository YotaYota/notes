# Users and Groups

- _/etc/passwd_
- _/etc/group_
- _/etc/shadow_

## _/etc/passwd_

```
<user name>:x:<id of user>:<id of primary group>:<description of user>:<home dir>:<login shell>
```

## _/etc/group_

```
<group name>:x:<id of group>:<members>
```

## _/etc/shadow_

```
<user name>:<password hash>:<password meta data>
```

### *password hash*

- If starts with ! it denotes that the account is locked
- If it is onlt an asterix \* it means the user cannot be logged into with a password

## Users

- normal users
- root users
    - the *root* user
    - normal users with sudo access

Create user with home directory (`-m`)

```
usermod -m <user name>
```

*Note*: prepopulated with stuf from _/etc/skel_.

Change shell (`-s`) of user

```
usermod -s /bin/bash <user name>
```

Change password of user

```
passwd <user name>
```

Lock user

```
passwd -l <user name>
```

Unlock user

```
passwd -u <user name>
```

### Sudoers

Must use `visudo`

```
visudo /etc/sudoers
```

## Groups

`groupadd`, `groupdel`

Append user to group

```
usermod -a -G <group name> <user name>
```

