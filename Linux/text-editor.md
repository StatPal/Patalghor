# Text editors

## Beginners
- For beginners with **Graphical User Interface(GUI)**, I would suggest to work with [gedit](https://wiki.gnome.org/Apps/Gedit)/[kate](https://kate-editor.org/) first. (For common linux distributions, one of these two are there depending on our desktop environment.) With these editors, you can install many plug-ins, such as word-completion, for better workflow. Another useful GUI based editor would be [notepad++](https://notepad-plus-plus.org/) (windows version) with linux version - **notepadqq**. 
- To work with **command line tool**, you can work with [nano](https://nano-editor.org/) or [micro](https://micro-editor.github.io/) editor. I would suggest to get the basics of `nano` first, as it is available in most linux machines. The problems with *nano* is that it has little bit non-conventional keybindings in older versions, such as `ctrl+x` for exit `ctrl+g` for help, etc. Many of these are not-so-obvious at first. *micro* have somehow better keyinding - but it might not be available in many old machines, espceially if you are not admin of the machine.

## Intermediate advanced
For intermediate-advanced frequent user, I would suggest to learn [vim](https://www.vim.org/)/[emacs](https://www.gnu.org/software/emacs/)/[kakoune](https://kakoune.org/). Keep in mind that vim and kakoune are *modal editors*, i.e., these have different ~~moods~~ modes - where different modes are mainly for different kind of workflows - different keybindings work differently in different modes. 
Modes usually include, 
- *normal mode* (basic commands to do things like copying, pasting, deleting, other manipulations, moving cursor etc), 
- *insert mode* (to give inputs/type new things - `i` to go to this mode, `Esc` to come back to normal mode), 
- *search mode* etc. 

To learn *Vim*, you may try `vimtutor` or `gVim`(see later); to learn `kakoune`, you may try [trampoline](https://github.com/mawww/kakoune/blob/master/contrib/TRAMPOLINE); to learn *Emacs*, you may try [emacs tour](https://www.gnu.org/software/emacs/tour/). To shift between modes easily, consider this [vim-clutch](https://github.com/alevchuk/vim-clutch) - kidding, try `Esc` to come back to normal mode! However, *emacs* is a non-modal editor. 
*vim*, *emacs* is pre-installed to many linux computers and are much older with lots of support and I found it's painless to install kakoune for beginner-intermediate user even without admin access. 

There are other advanced, not-so-texty, GUI based editors [sublime text](https://www.sublimetext.com/), [visual studio code](https://code.visualstudio.com/), [atom](https://atom.io/), [gVim](https://www.vim.org/download.php)
However, sublime is not a fully-free software and atom/vs-code might be little slow in older computers as these are somewhat browser-based editors. gVim is Vim with a graphical user interface. Also, on `ssh` remote connections, these would not work with the usual default settings. 


------
Almost all these editors I suggested, they have features such as autosuggestion, bracket compleetion, code comment, using terminal commands and put it in file directly, change colorscheme according to coding language, file browsing, git color integration(not for all editors), multiple pane(not for all of the editors directly) or buffer *with or without plugins*.

I would suggest for a user to read https://andreyorst.gitlab.io/posts/2020-04-29-text-editors/ for comparison. Though I might be biased twords some of these editors, I think this is a good comparison between text-editors for an intermediate to advanced user.


------
## Specific Editors
### gedit
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


### nano
nano is a simple non-modal editor. 
To open one file in nano, type
```bash
nano <filename>
```
To open multiple files, write
```bash
nano <file1> <file2>
```

Basic shortcuts/keybindings are: 
(Ctrl is denoted as `^`, Meta (Alt) is denoted as `M`, Shift is denoted as `Sh`.
`Some key + another key` means you have to hold one key and then press another. 
Type `Ctrl+G` to get shortcuts and alternative shortcuts in your machine.
Without paranthesis means explanation in simple usual terms.
New correspondos to these are also added in new versions)
```
## File access
^G  Ctrl+G    Help
^X  Ctrl+X    Exit
^O  (^S new)  Write/Save
^R            Inser another file in current buffer (One have to type the new file name after ^R)

## Search and replace
^W (F6)       Find (in forward direction) [i.e., ^W, then type what you want to find, then enter]
^\            Replace a string (or rgular expression)

## Cut 'n paste
^K            Cut current line (or marked region) and store in cutbuffer
^U            paste contents of cutbuffer
M-6 Alt+6     Copy current line (or marked region) and store it in cutbuffer
M-A Alt+A     Mark text starting from the cursor position

# Undo Redo
M-U Alt+U     Undo
M-E Alt+G     Redo

## Navigation: 
^C            Display current position of the cursor
^/            Go to line and column number [i.e., type ^/ then line no (then column number if you want) then enter]
left arrow    back one character
right arrow   forward one character
up arrow      up one line
down arrow    down one line
^up arrow     Go to previous block of text
^down arrow   Go to next block of text

M-]           Go to the matching bracket


```

You can bind or unbind keys in `~.nanorc` or `~/.config/nano/nanorc` or corresponding file. 

###### colors
By default, the colorschemes are not loaded by default. In a nano configuration file named `.nanorc`, typically located in the home directory (~/.nanorc or ~/.config/nano/nanorc or $XDG_CONFIG_HOME/nano/nanorc), put `include /usr/share/nano/*.nanorc` for all default available languages, or `include /usr/share/nano/{lang}.nanorc` for any specific languages - see [this](https://askubuntu.com/questions/90013/how-do-i-enable-syntax-highlighting-in-nano). 

For more customization, see [this](https://www.nano-editor.org/dist/latest/nanorc.5.html) is the official website, also see [this](https://github.com/scopatz/nanorc) and [this](https://github.com/Naereen/nanorc) and [this](https://github.com/sentientmachine/erics_nano_syntax_highlighting) - some of these are for older versions. 


