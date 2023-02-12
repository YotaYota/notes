# Git

`git init` creates 2 things:

- **repository**
- **workspace**

`git init --bare` create only the **repository**.

---

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

## Workspaces

```
git clone <REPOSITORY> --bare
```

Add workspace with

```
git worktree add <BRANCH_NAME>
```

Will create a new workspace folder in the git bare repository.
