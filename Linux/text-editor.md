# Text editors

For beginners, I would suggest to work with [gedit](https://wiki.gnome.org/Apps/Gedit)/[kate](https://kate-editor.org/) first. (For common linux distributions, one of these two are there depending on our desktop environment.) These are simple GUI editors. However, you can install many plug-ins, such as word-completion for better workflow. Another useful GUI based editor would be [notepad++](https://notepad-plus-plus.org/) for windows version which has a linux version also - **notepadqq**. 

To work with **command line tool**, you can work with [nano](https://nano-editor.org/) or [micro](https://micro-editor.github.io/) editor. 
The problems with *nano* is that it has little bit non-conventional keybindings, such as `ctrl+x` for exit `ctrl+g` for help, etc. Many of these are not-so-obvious at first. *micro* have somehow better keyinding - but it might not be available in many old machines, espceially if you are not admin of the machine. *nano* is pre-installed in many common linux-distro - which is an advantage of nano. 

For intermediate-advanced frequent user, I would suggest to learn [vim](https://www.vim.org/)/[emacs](https://www.gnu.org/software/emacs/)/[kakoune](https://kakoune.org/). Keep in mind that vim and kakoune are *modal editors*, i.e., these have different ~~moods~~ modes where different modes is mainly for different kind of workflows. Modes include, *normal mode*(basic commands to do things like copying, pasting, deleting, other manipulations, moving cursor etc), *insert mode*(to give inputs/type new things), *search mode* etc. To shift between modes easily, consider this [vim-clutch](https://github.com/alevchuk/vim-clutch). However, *emacs* is a non-modal editor. 

*vim*, *emacs* is pre-installed to many linux computers and I found it's painless to install kakoune for beginner-intermediate user. 

There are other advanced, not-so-texty, GUI based editors [sublime text](https://www.sublimetext.com/), [visual studio code](https://code.visualstudio.com/), [atom](https://atom.io/)
However, sublime is not a fully-free software and atom/vs-code might be little slow in older computers as these are somewhat browser-based editors. 


Almost all these editors I suggested, they have features such as autosuggestion, bracket compleetion, code comment, using terminal commands and put it in file directly, change colorscheme, file browsing, git integration, multiple pane(not for all of the editors though) or buffer with or without plugins.

I would suggest for a user to read https://andreyorst.gitlab.io/posts/2020-04-29-text-editors/ for comparison. Though I might be biased twords some of these editors, I think this is a good comparison between text-editors for an intermediate to advanced user.



## gedit plugins
Gedit is a 'what I see is what I type' editor. Some plug-ins I use frequently are:
- Bookmarks
- Bracket Completion
- Code comment
- Embedded Terminal
- File Browser Panel
- Git
- Quick Highlight
- Quick Open
- Spell Checker
- Word Completion. 
