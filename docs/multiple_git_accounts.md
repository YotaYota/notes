# SSH Host Aliases

Use ssh keys and define host aliases in ssh config file (each alias for an account).

## Steps

1. [Generate ssh key pairs for accounts](https://help.github.com/articles/generating-a-new-ssh-key/)
and [add them to GitHub accounts](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
2. Edit/Create ssh config file (`~/.ssh/config`):

```conf
# Default github account: firstAccountNames
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/oanhnn_private_key
  IdentitiesOnly yes

# Other github account: secondAccountName
Host github-secondAccountName
  HostName github.com
  IdentityFile ~/.ssh/superman_private_key
  IdentitiesOnly yes
```

3. [Add ssh private keys to your agent](https://help.github.com/articles/adding-a-new-ssh-key-to-the-ssh-agent/):

```shell
$ ssh-add ~/.ssh/oanhnn_private_key
$ ssh-add ~/.ssh/superman_private_key
```

4. Test your connection

```shell
$ ssh -T git@github.com
$ ssh -T git@github-secondAccountName
```

   With each command, you may see this kind of warning, type `yes`:

```shell
The authenticity of host 'github.com (192.30.252.1)' can't be established.
RSA key fingerprint is xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:
Are you sure you want to continue connecting (yes/no)?
```

   If everything is OK, you will see these messages:

```shell
Hi firstAccountNames! You've successfully authenticated, but GitHub does not
provide shell access.
```

```shell
Hi secondAccountName! You've successfully authenticated, but GitHub does not
provide shell access.
```

5. Now all are set, just clone your repositories

```shell
$ git clone git@github-secondAccountName:org2/project2.git /path/to/project2
$ cd /path/to/project2
$ git config user.email "secondAccountName@org2.com"
$ git config user.name  "Super Man"
```

