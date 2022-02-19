# nvimdev.nvim

Provides some nicities for hacking on [Neovim][]:

- Auto-detect Neovim source tree and `:cd` to the root.
- Build `tags` on save.
  - Configured to work well with the generated sources.
- Build `cscope.out` on save.
  - Commands for looking up callers and callees from the current function.
- Fast linting for C sources (clint.py).
  - Ignored errors list can be fetched/updated via `:NvimUpdateClintErrors`.
  - Uses `vim.diagnostic`
- Configures Neomake's luacheck maker to use the version from the `.deps`
  directory, if it is not available otherwise.
- Filetype settings appropriate for Neovim's source code.
- Hook into [vim-projectionist]: configure alternate files for the ".vim-src"
  directory, and a command to diff against the same file in Vim.
- Add commands `NvimTestRun` and `NvimTestClear` for running functional tests directly in the buffer.


## Why?

Neovim has a pretty large code base and is full of Vim's rich and mysterious
history.  I have little knowledge of either and [wrote a script][gist] to help
me get around and automate some things.  People have shown interest in using
it, so it's now a plugin that will make maintenance easier.

## Installation

😑

## Requirements

- [Neomake][]
- Python 3
- [plenary.nvim][]

## Config

#### `g:nvimdev_auto_init` (default `1`)

Automatically enable when within the Neovim source tree, or one of its files
are loaded.

It can be manually enabled with:

```vim
call nvimdev#init("path/to/neovim")
```

It should always be set to the actual Neovim root.

#### `g:nvimdev_auto_cd` (default `1`)

Automatically `:cd` to the Neovim root after init.

#### `g:nvimdev_auto_ctags` (default `0`)

Automatically generate tags.

For best results, use [universal-ctags][].

#### `g:nvimdev_auto_cscope` (default `0`)

Automatically generate `cscope.out`.  Requires [cscope][].

The following commands will be added to `.c` buffers:

- `:Callers` - displays a list of locations calling a function
- `:Callees` - displays a list of functions called by a function

Each command takes an optional name.  Without arguments, the name of the
function at the cursor location is used.

If you want to display the results as quickfix items:

```vim
set cscopequickfix=s-,c-,d-,i-,t-,e-,a-
```

**Disclaimer:** I don't use `cscope` much and this didn't turn out as awesome
as I hoped it would.  Using the commands above, or `:cscope` with quickfix
enabled will cause the current buffer to jump to the first match.  Also, it
results in a bunch of duplicates since I think `cscope` indexes functions with
any aliased return types it finds.  I'm tempted to write a dumb script that
uses the `tags` file to build a cross refrence database.


#### `g:nvimdev_auto_lint` (default `0`)

Adds nvim linter to `g:neomake_c_enabled_makers`.

#### `g:nvimdev_build_readonly` (default `1`)

Set files loaded from `build/` or `.deps/` as readonly.

## Commands

`NvimTestRun [all]`: Run the test in the buffer the cursor is inside. Works for `it` and `describe` blocks.

`NvimTestClear`: Clear test result decorations in buffer

## Useful plugins

- [nvim-cmp][]: Completions!
- [cmp-nvim-lsp][]: LSP completion source!
- [nvim-lspconfig][]: LSP configuration for [clangd][] and [sumneko-lua-lsp][].
- [nvim-treesitter][]: Better syntax highlighting for C and Lua files.
- [lua-dev.nvim][]: Neovim Lua development.

[Neovim]: https://github.com/neovim/neovim
[Neomake]: https://github.com/neomake/neomake
[universal-ctags]: https://github.com/universal-ctags/ctags
[cscope]: http://cscope.sourceforge.net/
[nvim-treesitter]: https://github.com/nvim-treesitter/nvim-treesitter
[nvim-cmp]: https://github.com/hrsh7th/nvim-cmp
[cmp-nvim-lsp]: hrsh7th/cmp-nvim-lsp
[nvim-lspconfig]: https://github.com/neovim/nvim-lspconfig
[sumneko-lua-lsp]: https://github.com/sumneko/lua-language-server
[clangd]: https://clangd.llvm.org
[gist]: https://gist.github.com/tweekmonster/8f9cfb36a56d7d1bb6a73e0f9589d81f
[vim-projectionist]: https://github.com/tpope/vim-projectionist
[plenary.nvim]: https://github.com/nvim-lua/plenary.nvim
[lua-dev.nvim]: https://github.com/folke/lua-dev.nvim
