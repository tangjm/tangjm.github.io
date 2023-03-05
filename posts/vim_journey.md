--- 
title: 'Vim journey'
date: '2023-03-05'
--- 

### Vim journey

Some thoughts on vim at various points in time.

May/June 2021 

Started programming with VSCode and Git Bash.

July 2022

Have heard of vim before but decided to learn basic Vim motions to make it easier to work in CentOS on my first client deployment.
The vimtutor tutorial was instructive and didn't take too long to work through. After a couple of hours I was comfortable with the basics. The most immediately useful commands were hjkl for moving around as well as the :s and :%s commands for making replacements. 

Installed the vim emulator plugin in VSCode and this is now my current setup.

November 2022

I picked up tmux and more vim motions.
Considered moving from VSCode to Vim for my main text editor for coding activities but quickly returned to VSCode due to the lack of syntax highlighting, autocompletion, efficient file search and other features provided by VSCode.

03/12/2022

Learnt about a cool alternative for quitting the vim text editor.
- ZZ is equivalent to :wq
- ZQ is equivalent to :q!

Some advantages of using vim that I can personally vouch for:

- ubiquity. It is really convenient to be able to use vim on linux servers. 
- accuracy. One of the biggest advantages is the amount of control you have over text editing. I have found vim to be more accurate than using a mouse for highlighting text and for navigating around source code and less frustrating than using a mouse.

Why I am hesitant to move to vim for coding:

- VSCode is more efficient for opening multiple files and working with a larger project. I have yet to explore other vim features so until I figure out how to use vim buffers and how to install plugins I will keep open the option of using vim for coding. But for now I will stick to VSCode. 

4-6 Feb 2023 

Setup relative line numbers for easier nagivation in NORMAL mode as I no longer have to do mental arithmetic to determine how to jump up or down my screen.

Learnt about vim buffers/windows/tabs. 

New key combinations discovered:
- 'zt' and 'zb' work similar to 'zz' and move the current line your cursor is on to the top and bottom of your screen.
- Shift+H, Shift+M and Shift-L move your cursor to the highest, middle and lowest visible lines on your screen.
- 'C-a' increments the number under the cursor.
- 'C-x' decrements the number under the cursor.
- 'gd' takes you to the local declaration of the variable under the cursor.
- 'gD' takes you to the global declaration of the variable under the cursor.

n.b. '\<C-a\>' stands for Ctrl + a.

Endeavour to use 'change' instead of 'delete' where possible.
- Learnt about the 'i' and 'a' modifiers and how they can work with quotes, angle tags and brackets.

Installed my first two vim plugins 
- NERDTree (File system explorer for Vim)
- ctrlp (Fuzzy finder for Vim; cf. ctrl+p in VS Code) 

Setup Neovim.

Built and installed Neovim stable version.
Setup neovim plugins with Lua.

Plugins 
- telescope (Fuzzy finder like 'ctrlp')
- tree-sitter (On the fly syntax highlighting achievable through incremental parsing)
- undotree (Visualise undo history)
- rose-pine (Colour theme)
- git-fugitive (Git VCS wrapper within nvim)
- lsp-zero (LSP features - Brings IDE features like code completion, syntax warnings and language specific features to vim.)

7 Feb 2023

- Made several key remaps.
- Setup Neovim. 
- Setup neovim runtime config at $HOME/.config/nvim/

14 Feb 2023 

'vip', 'dip', 'cip' and their analogues 'vap', 'dap', 'cap' let you select/delete/change inside or around a paragraph.

16 Feb 2023 

Realised how much more efficient 'cib' is compared to 'dib' even though it only saves a single keystroke as it means I can immediately start typing without remembering to first enter INSERT mode.

4 March 2023

Setup line numbers and intuitive formatting and indentation settings for neovim. This has made editing code in neovim a much closer experience to doing the same in VSCode. I still find VSCode preferable and often resort to opening a project in VSCode instead of neovim. But for one-off edits, bash scripting and editing dotfiles, I now use neovim over vim and VSCode.  

The biggest advantage of neovim over vim is the Language Server Protocol plugin which effectively adds IDE features found in VSCode and IntelliJ to vim, putting it on a par with IDEs. I do think this makes for a viable code editor. In the coming weeks I will try and spend more time coding in neovim.

The startup speed of neovim is also incredibly fast and there is no wait time compared to an IDE like VSCode. This together with support for vim motions is one area in which neovim outshines VSCode.

My current problems with neovim: 

- Default yanking behaviour doesn't copy to the system clipboard due to pecularities with WSL2. I am currently using vim for cases where I need to copy to the system clipboard and will need to find a way to replicate this fix in neovim. 
- Will need to find a debugger plugin for debugging purposes. This is one area where I think IDEs are superior.
