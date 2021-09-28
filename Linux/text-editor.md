# Text editors

- [Beginners](#beginners)
- [Intermediate advanced](#intermediate-advanced)
- [Specific Editors](#specific-editors)
  * [gedit](#gedit)
      - [Plug ins](#plug-ins)
  * [nano](#nano)
      - [Basic shortcuts or keybindings](#basic-shortcuts-or-keybindings)
        * [File access](#file-access)
        * [Search and replace](#search-and-replace)
        * [Cut 'n paste](#cut-n-paste)
        * [Undo Redo](#undo-redo)
        * [Navigation:](#navigation)
        * [Delete](#delete)
        * [Indenting, commenting etc](#indenting-commenting-etc)
        * [File buffers](#file-buffers)
        * [More (intermediate user)](#more-intermediate-user)
        * [Create Keybinds (intermediate user)](#create-keybinds-intermediate-user)
      - [Adding colors](#adding-colors)



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
##### Plug ins 
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


<hr style="border:1px solid blue"> </hr>

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
You can then go from one file to another using the shortcuts/keybinds `Alt+rightarrow`or `Alt+leftarrow`. 

##### Basic shortcuts or keybindings 

(<sub>**Ctrl** is denoted as **`^`** which is equivalent to double Esc key, *Meta* (usually **Alt**, but can be Esc/Cmd key also) is denoted as **`M`**, Shift is denoted as `Sh`.</sub>

<sub>`Some key + another key` means you have to hold one key and then press another.</sub> 

<sub>Type `Ctrl+G` to get shortcuts and alternative shortcuts in your machine.</sub>
<sub>Without paranthesis means explanation in simple usual terms.</sub>
<sub>New correspondos to these are also added in new versions</sub>)

###### File access
```
^G  Ctrl+G    Help
^X  Ctrl+X    Exit
^O            Write/Save
^S  new       Save without prompting 
^R            Insert another file in current buffer (One have to type the new file name after ^R)
```
`Ctrl+C` is cancel in most of the cases, i.e., you did  to open a file

###### Search and replace
```
^W (F6)       Where is: Find (in forward direction) [i.e., ^W, then type what you want to find, then enter]
^Q            Where was: Find (in the backward direction)
M-Q           Search next occurrence backward
M-W           Search next occurrence forward
^\            Replace a string (or rgular expression)
```

###### Cut 'n paste
```
^K            Cut current line (or marked region) and store in cutbuffer
^U            paste contents of cutbuffer
M-6 Alt+6     Copy current line (or marked region) and store it in cutbuffer
M-A Alt+A     Mark text starting from the cursor position
```

###### Undo Redo
```
M-U Alt+U     Undo
M-E Alt+G     Redo
```

###### Navigation: 
```
^C            Display current position of the cursor
^/            Go to line and column number [i.e., type ^/ then line no (then column number if you want) then enter]
left arrow    back one character
right arrow   forward one character
up arrow      up one line
down arrow    down one line
^up arrow     Go to previous block of text
^down arrow   Go to next block of text
M+]           Go to the matching bracket
Home  (^A)    Go to beginning of current line
End   (^E)    Go to end of current line
```

###### Delete
```
Bsp     (^H)      Delete the character to the left of the cursor
Del     (^D)      Delete the character under the cursor
M-Bsp   (Sh+^Del) Delete backward from cursor to word start
^Del              Delete forward from cursor to next word start
M+Del             Throw away the current line (or marked region)
```

###### Indenting, commenting etc:
```
Tab       (M+})   Indent the current line (or marked lines)
Sh+tab    (M+{)   Unindent the current line (or marked lines)
M+3               Comment/uncomment the current line (or marked lines)
^]                Try and complete the current word
M-D               Count the number of words, lines, and characters
```

###### File buffers
If you open more than one file at one - they are basically different buffers. However there can be more complicated scenarios. 
```
M-left arrow  (M->) (M-.)    Go to previous buffer (opened file)
M-right arrow (M-<) (M-,)    Go to the next buffer (opened file)
```

###### More (intermediate user)
Anchor/Bookmark some line and navigate:
```
M-Ins            Place or remove an anchor at the current line
M-PgUp           Jump backward to the nearest anchor
M-PgDn           Jump forward to the nearest anchor
```
To execute some command from:
```
^T               -- then type the shell command and enter
```
Like if you type `^T` and then `date`(which is a shell command, producing today's date), it will print today's date in current cursor position. 
However, this might be problematic if Ctrl+T is already used for some other purpose in your machine. In that case you might bind a new key for this. (See the next paragraph)
Or, you can do `^R` then `^X` to get this facility of executing commnds. 

To get an idea about which keybinds are usually more usueful, you may look at [this link](https://staffwww.fullcoll.edu/sedwards/Nano/NanoKeyboardCommands.html)

###### Create Keybinds (intermediate user)
You can create your own shortcuts/keybinds in most of these text editors.  
For example, to exectue a (linux/shell) command and get the output printed inside nano suppose I want to bind `Shift-Alt-E`. Then I should include this next line in `~.nanorc` or `~/.config/nano/nanorc` or corresponding file. 
```
bind Sh-M-E execute main
```
Then I in usual nano session, I have to `Shift+Alt+E` and type a (shell) command and type enter to get that result.
There are already some keybindings you can see at `/etc/nanorc`

For quickly uppercasing or lowercasing the word under the cursor.
```
bind Sh-M-U "^[Oc^[[1;6D^T|sed 's/.*/\U&/'^M" main
bind Sh-M-L "^[Oc^[[1;6D^T|sed 's/.*/\L&/'^M" main
```

There are another important set called `macro` which can record a set of keys and run them subsequently. For more, look at help files. 


##### Adding colors
By default, the colorschemes are not loaded by default. In a nano configuration file named `.nanorc`, typically located in the home directory (~/.nanorc or ~/.config/nano/nanorc or $XDG_CONFIG_HOME/nano/nanorc), put `include /usr/share/nano/*.nanorc` for all default available languages, or `include /usr/share/nano/{lang}.nanorc` for any specific languages - see [this](https://askubuntu.com/questions/90013/how-do-i-enable-syntax-highlighting-in-nano). 

For more customization, see [this](https://www.nano-editor.org/dist/latest/nanorc.5.html) is the official website, also see [this](https://github.com/scopatz/nanorc) and [this](https://github.com/Naereen/nanorc) and [this](https://github.com/sentientmachine/erics_nano_syntax_highlighting) - some of these are for older versions. 


##### Some more settings
People sometimes `set/unset` things according to their preferences, such as:

```
set tabsize 4
# set linenumbers
set historylog                          # remember search history
set autoindent


set zap                                 # Let an unmodified Backspace or Delete erase the marked region (instead of a single character, and without affecting the cutbuffer).
set softwrap                            # Display lines that exceed the screen’s width over multiple screen lines.
set atblanks                            # When soft line wrapping is enabled, make it wrap lines at blank characters (tabs and spaces) instead of always at the edge of the screen.
set positionlog                         # Save the cursor position of files between editing sessions for 200 recent files

#set minibar
#set minicolor italic,peach,gray

set titlecolor yellow,gray
set functioncolor yellow
set keycolor lightyellow


## https://gist.github.com/jyc/1375240
set brackets ""')>]}"                   # The characters treated as closing brackets when justifying paragraphs. They cannot contain blank characters.  Only closing punctuation, optionally followed by closing brackets, can end sentences.
set matchbrackets "(<[{)>]}"            # The opening and closing brackets that can be found by bracket searches. They cannot contain blank characters.  The former set must come before the latter set, and both must be in the same order.


set speller "aspell -x -c"              # Use this spelling checker instead of the internal one.  This option does not properly have default value.
```
