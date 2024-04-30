# Nano

`nano` is a simple editor. 

If you need minimal work in a text editor in Linux - `nano` is an excellent choice for beginners without a lot of learning curve. For most unix-based systems, it is already installed. I'm showing everything in recent versions of nano.
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
- use Ctrl+G or F1 for help


## Other common commands:
- Find: `Ctrl + W`
- Replace: `Ctrl + \`
- Cut a line (or marked region): `Ctrl + K`
- paste cut-buffer: `Ctrl + U`
