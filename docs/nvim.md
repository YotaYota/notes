# Neovim
  
## LSP

Neovim supports LSP (Language Server Protocol); ie it acts as a client to LSP
servers. Nvim includes a Lua framework `vim. lsp` for building enhanced LSP tools.

**Note**: LSP facilitates features like go-to-definition, find-references, hover,
completion, rename, format, refactor, etc., using semantic whole-project
analysis.

```lua
use {
  "williamboman/mason.nvim",           -- language server installer
  "williamboman/mason-lspconfig.nvim",
  "neovim/nvim-lspconfig",             -- configs for Nvim LSP client
}
```

### mason.nvim

`mason.nvim` installs packages to nvim's `:h stdpath`.

### mason-lspconfig

`mason-lspconfig` bridges Mason with the `lspconfig` plugin.

- [Available servers](https://github.com/williamboman/mason-lspconfig.nvim#available-lsp-servers)

**Note** mason-lspconfig uses `lspconfig` server names, **not** `mason.nvim` package names. [List of mappings](https://github.com/williamboman/mason-lspconfig.nvim/blob/main/doc/server-mapping.md)

### null-ls

```lua
use "jose-elias-alvarez/null-ls.nvim"
```

[Build-in Sources](https://github.com/jose-elias-alvarez/null-ls.nvim/blob/main/doc/BUILTINS.md)

To run built-in sources, the command specified below must be available on your
`$PATH` and visible to Neovim. Eg, to check if `stylua` is available, run the
following (Vim, not Lua) command:

```vim
" should echo 1 if available (and 0 if not)
:echo executable("stylua")
```
