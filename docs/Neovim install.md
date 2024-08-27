# Noevim Install


> note: 实用技巧 https://xu3352.github.io/linux/2018/10/16/practical-vim-skills

`https://xu3352.github.io/practical_vim`




```bash
brew install nvim
mv ~/.config/nvim ~/.config/nvim.backup
rm -rf ~/.config/nvim 
rm -rf ~/.local/state/nvim 
rm -rf ~/.local/share/nvim

git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```



```sh
rm -rf ~/.config/nvim && rm -rf ~/.local/state/nvim && rm -rf ~/.local/share/nvim
```

选项
`lua/options.lua`

```lua
local o = vim.o

o.relativenumber = true
o.tabstop = 4
o.shiftwidth = 4
o.wrap = false
```

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