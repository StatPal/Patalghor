# Nano

`nano` is a simple editor. 

If you need minimal work in a text editor in Linux - `nano` is an excellent choice for beginners without a lot of learning curve. For unix-based systems, it is already installed. This is for recent versions of nano.
Enjoy...

## First step of nano:
To just open a nano instance without having a file in mind:
- In the terminal, write `nano`
- Then write something
- Press `ctrl + o` or `ctrl + s` to save and write the desired NAME of the FILE.
- Press `ctrl + x` to exit.

If you want to open a file some/path/abc.txt, and change it:
- In the terminal, write `nano some/path/abc.txt`
- Then write/change something
- `ctrl + o` or `ctrl + s` to save (same file name)
- `ctrl + x` to exit

You can press `ctrl + x` without the saving `ctrl + s` as well. In that case, nano will ask you `Save modified buffer?` with the opotions `Y - yes`, `N - no`, `ctrl + c - cancel`.
Then you can press `Y` to save the file (or `N` not to save it and exit or `ctrl + c` to go back to the editing phase without exiting.) 


#### Tips
Most of the shortcuts are already written when you enter nano at the bottom.
- ^ usually means ctrl
- M usually means alt
- use `Ctrl+G` or `F1` for help


## Other common commands (tried to write more common functions first):
- Find: `Ctrl + W`
- Replace: `Ctrl + \`
- Cut a line (or marked region): `Ctrl + K`
- paste cut-buffer: `Ctrl + U`
- Undo: `alt + U`
- Redo: `Alt + E`
- Movecursor through words/paragraphs at once: `Ctrl + arrow keys`
- Comment/Uncomment the line/marked selection: `Alt+ 3`
- If multiple files are open (for example, using `nano abc.txt bcd.py cde.R`), switch between buffers: `Alt + left/right arrow`
- Soft wrap: `Alt + S`
- Delete word to the left: `Alt + Backspace`
- Delete word to the right: `ctrl + delete`
- Complete current word: `ctrl + ]`
- Copy a line (or marked region): `Alt + 6`
- Go to complementary bracket: `Alt + ]`
- Report cursor position: `ctrl + c`

## Intermediate things:
#### File browser:
When in the Read-File (`Ctrl + R`) or Write-Out menu (`Ctrl + O`), pressing (`ctrl + t`) will invoke the file browser. 
Here, one can navigate directories in a graphical manner in order to find the desired file.


#### Execute bash commands:
Suppose you want to put the output of some bash command (e.g., `date`) in the current line. You can execute the command by:
- `ctrl + t`
- Type your command (such as `date` or `ls` etc)
- enter
Voila!

#### regex


#### nanorc and Color schemes
The nanorc file consists of the modifiable settings of nano, including colors corresponding to filetypes. Some common color schemes are already in your `/usr/share/nano/` folder and you might have to load them (by `include "/usr/share/nano/nanorc.nanorc"`, `include "/usr/share/nano/c.nanorc"` etc.) Usually your nanorc is either in home directory or in `~/.config/nano/`

My complete nanorc, nanorc for R and matlab (for color scheme) is attached later

#### Rebind keys

#### Macros

#### Mouse activity:

To know more, go to: https://www.nano-editor.org/dist/latest/nano.html. 

----------------------------
### Footnote
`~.config/nano/nanorc` file:

```
include "/usr/share/nano/nanorc.nanorc"
include "/usr/share/nano/nanohelp.nanorc"
include "/usr/share/nano/default.nanorc"

include "/usr/share/nano/c.nanorc"
include "/usr/share/nano/python.nanorc"
include "/usr/share/nano/lua.nanorc"
include "/usr/share/nano/man.nanorc"
include "/usr/share/nano/markdown.nanorc"
include "/usr/share/nano/makefile.nanorc"
include "/usr/share/nano/html.nanorc"
include "/usr/share/nano/css.nanorc"
include "/usr/share/nano/email.nanorc"
include "/usr/share/nano/changelog.nanorc"
include "/usr/share/nano/sh.nanorc"
include "/usr/share/nano/rust.nanorc"
include "/usr/share/nano/tex.nanorc"
# include "/usr/share/nano/textinfo.nanorc"
include "/usr/share/nano/extra/fortran.nanorc"


include "~/.config/nano/my_r.nanorc"
include "~/.config/nano/my_octave.nanorc"



## Keybinds: -------------------------------------------------------

# For quickly uppercasing or lowercasing the word under the cursor.
bind Sh-M-U "^[Oc^[[1;6D^T|sed 's/.*/\U&/'^M" main
bind Sh-M-L "^[Oc^[[1;6D^T|sed 's/.*/\L&/'^M" main




set tabsize 4
# set linenumbers
set historylog							# remember search history
set autoindent


set zap									# Let an unmodified Backspace or Delete erase the marked region (instead of a single character, and without affecting the cutbuffer).
set softwrap							# Display lines that exceed the screen’s width over multiple screen lines.
set atblanks 							# When soft line wrapping is enabled, make it wrap lines at blank characters (tabs and spaces) instead of always at the edge of the screen.
set positionlog							# Save the cursor position of files between editing sessions for 200 recent files

#set minibar
#set minicolor italic,peach,gray

set titlecolor yellow,gray
set functioncolor yellow
set keycolor lightyellow


## https://gist.github.com/jyc/1375240
set brackets ""')>]}"					# The characters treated as closing brackets when justifying paragraphs. They cannot contain blank characters.  Only closing punctuation, optionally followed by closing brackets, can end sentences.
set matchbrackets "(<[{)>]}"			# The opening and closing brackets that can be found by bracket searches. They cannot contain blank characters.  The former set must come before the latter set, and both must be in the same order.


set speller "aspell -x -c"				# Use this spelling checker instead of the internal one.  This option does not properly have default value.
```

##### R color scheme in `~.config/nano/my_r.nanorc`: 
```
# https://www.reddit.com/r/linux/comments/b724zx/gnunano_looks_awesome_with_syntax_highlight_and/

# # From this website:
# # set element fgcolor,bgcolor
# set titlecolor brightwhite,blue
# set statuscolor brightwhite,green
# set errorcolor brightwhite,red
# set selectedcolor brightwhite,magenta
# set stripecolor ,yellow
# set numbercolor cyan
# set keycolor cyan
# set functioncolor green

# #^ Set is not permitted in included file -- change this later.

syntax "R" "\.(R|RDATA|RData)$"
# color brightwhite "\<[A-Z_][0-9A-Z_]+\>"


color magenta "\<(for|if|while|do|else|case|default|switch|break|return|ifelse|try|catch|which|function)\>"
color brightyellow "\<(TRUE|FALSE|T|F)\>"
color brightyellow "\<(library)\>"

## Braces:
# color brightyellow "\<\>"
color green "(\{|\}|\(|\)|\;|\]|\[|`|\\|\$|<|>|!|=|&|\|)"
## copied from bash rc

## equality sign:
color yellow "(\:|\$|<-|->|!|=|\&|\|)"

color black "(\;)"



## All numbers:
color green "\<[0-9]+\>"
# brightgreen makes everything more bold kind of thing!

## String highlighting.  You will in general want your comments and
## strings to come last, because syntax highlighting rules will be
## applied in the order they are read in.
color green "<[^=        ]*>" ""(\\.|[^"])*""
## This string is VERY resource intensive!
color green start=""(\\.|[^"])*\\[[:space:]]*$" end="^(\\.|[^"])*""
## ^Copied

## All commenting: would override anything - that's why it's at last.
# color magenta "#.*"
color blue "#.*"
## or brightblue/blue
```
##### Ocatave/matlab color scheme in `~.config/nano/my_octave.nanorc`: 
```
# Matlab/Octave syntax colors
syntax "octave" "\.m$" "\.octaverc$"

# keywords
color brightyellow "(case|catch|do|else(if)?|for|function|if|otherwise|switch|try|until|unwind_protect(_cleanup)?|vararg(in|out)|while)"
color brightyellow "end(_try_catch|_unwind_protect|for|function|if|switch|while)?"
color magenta "(break|continue|return)"

# storage-type
color green "(global|persistent|static)"

# data-type
color green "(cell(str)?|char|double|(u)?int(8|16|32|64)|logical|single|struct)"

# embraced
color brightred start="\(" end="\)"
color blue start="\[|\{" end="\]|\}"

# strings
color yellow ""(\\.|[^\"])*"|'(\\.|[^\"])*'"

# comments
color brightblue "#.*|%.*"
```
