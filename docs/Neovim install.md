# Noevim Install


> note: 实用技巧 https://xu3352.github.io/linux/2018/10/16/practical-vim-skills

`https://xu3352.github.io/practical_vim`




```bash
brew install nvim
mv ~/.config/nvim ~/.config/nvim.backup
rm -rf ~/.config/nvim 
rm -rf ~/.local/state/nvim 
rm -rf ~/.local/share/nvim

git clone https://github.com/cmacro/NvChad.git ~/.config/nvim && nvim
or
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

安装默认lsp（lua, http, css）
`MasonInstallAll`

```sh
rm -rf ~/.config/nvim && rm -rf ~/.local/state/nvim && rm -rf ~/.local/share/nvim
```
Obsidian
选项
`lua/options.lua`



```lua
local o = vim.o

o.relativenumber = true
o.tabstop = 4
o.shiftwidth = 4
o.wrap = false
```

nvim tree 打开git_ignored 过滤文件和文件夹

```sh
{
    "nvim-tree/nvim-tree.lua",
    opts = {
      filters = {
        git_ignored = false,
      },
    },
  },
```

> note: 更多参考 `https://github.com/nvim-tree/nvim-tree.lua/blob/master/doc/nvim-tree-lua.txt` 

快速跳转插件
`/lua/plugins/init.lua`

```lua
 {
        "phaazon/hop.nvim",
        branch = "v2", -- optional but strongly recommended
        opts = {
            multi_windows = true,
            keys = "etovxqpdygfblzhckisuran",
            upercase_labels = true,
        },
        keys = {
            {
                "<leader>fj",
                function()
                    require("hop").hint_words()
                end,
                mode = { "n", "x", "o" },
            },
        },
    },

```

![[Pasted image 20240828210545.png]]
##  语言高亮显示设置

https://github.com/nvim-treesitter/nvim-treesitter

加载 `nvim-treesitter` 插件 `plugins/init.lua` 增加配置

```lua
  {
    "nvim-treesitter/nvim-treesitter",
    event = { "BufWritePre", "BufNewFile" },
    config = function ()
      require("configs.treesitter")
    end,
  },
```


config 目录增加配置 `lua/configs/treesitter.lua`
```lua
local options = {
    ensure_installed = {
        "bash",
        "c",
        -- "cpp",
        "go",
        "gomod",
        "gosum",
        "gotmpl",
        "gowork",
        "lua",
        "luadoc",
        "make",
        "markdown",
        "python",
        "toml",
        "vim",
        "vimdoc",
        "yaml",
    },

    highlight = {
        enable = true,
        use_languagetree = true,
    },

    indent = { enable = true },
}

require("nvim-treesitter.configs").setup(options)
```


效果图

`:TSInstallInfo` 查看支持语言高亮的安装列表


![[Pasted image 20240828212625.png]]


## go lspconfig

com

```lua
go = { "gofumpt", "goimports-reviser", "golines" },
```

`lspconfig.lua`

```lua
lspconfig.servers = {
  -- "lua_ls",
  "gopls",
}

... example
... last add

lspconfig.gopls.setup({
  on_attach = function(client, bufnr)
    client.server_capabilities.documentFormattingProvider = false
    client.server_capabilities.documentRangeFormattingProvider = false
    nvlsp.on_attach(client, bufnr)
  end,
  on_init = nvlsp.on_init,
  capabilities = nvlsp.capabilities,
  cmd = { "gopls" },
  filetypes = { "go", "gomod", "gotmpl", "gowork" },
  root_dir = lspconfig.util.root_pattern("go.work", "go.mod", ".git"),
  settings = {
    gopls = {
      analyses = {
        unusedparams = true,
      },
      completeUnimported = true,
      usePlaceholders = true,
      staticcheck = true,
    },
  },
})
```


格式化设置 `conform.lua`

```lua
local options = {
  formatters_by_ft = {
    ...
    go = { "gofumpt", "goimports-reviser", "golines" },
  },
 
  formatters = {
    -- Golang
    ["goimports-reviser"] = {
      prepend_args = { "-rm-unused" },
    },
    golines = {
      prepend_args = { "--max-len=120" },
    },
  },
  
  ....
  format_on_save = {
    .....
  }
}
```
## go debug

go debug
`https://github.com/go-delve/delve`


TSInstall go

```lua
{
	"mfussenegger/nvim-dap",
},
{
	"leoluz/nvim-dap-go",
	ft = "go"
	dependencies = "mfussenegger/nvim-dap",
	config = 
	
},
```
## 插件

| name                 | note      |     |
| -------------------- | --------- | --- |
| olexsmir/gopher.nvim | 增加Sruct标签 |     |
