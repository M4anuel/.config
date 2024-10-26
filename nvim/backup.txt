vim.cmd("set expandtab")
vim.cmd("set tabstop=2")
vim.cmd("set softtabstop=2")
vim.cmd("set shiftwidth=2")
vim.cmd("set number")
vim.g.mapleader = " "

-- Bootstrap lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then
    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)


-- Setup lazy.nvim
require("lazy").setup({
  spec = {
        {
          "polirritmico/monokai-nightasty.nvim",
          lazy = false,
  	      priority = 1000,
      	  keys = {
	          { "<leader>tt", "<Cmd>MonokaiToggleLight<CR>", desc = "Monokai-Nightasty: Toggle dark/light theme." },
            },
          ---@type monokai.UserConfig
          opts = {
            dark_style_background = "default",
            light_style_background = "default",
            markdown_header_marks = true,
            -- hl_styles = { comments = { italic = false } },
            terminal_colors = function(colors) return { fg = colors.fg_dark } end,
          },
          config = function(_, opts)
          vim.opt.cursorline = true -- Highlight line at the cursor position
          vim.o.background = "dark" -- Default to dark theme

          require("monokai-nightasty").load(opts)
          end,
        },
        {
          'nvim-telescope/telescope.nvim', tag = '0.1.8',
            -- or                              , branch = '0.1.x',
          dependencies = { 'nvim-lua/plenary.nvim' }
        },
	-- Configure any other settings here. See the documentation for more details.
      checker = { enabled = true },
      }
})

local builtin = require("telescope.builtin")

vim.keymap.set('n', '<C-P>', builtin.find_files, {})
vim.keymap.set('n', '<leader>fg', builtin.live_grep, {})
