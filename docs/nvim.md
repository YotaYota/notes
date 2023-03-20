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
