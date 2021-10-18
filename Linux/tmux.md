*** **This is realtively advanced feature to divide and conquor a terminal. It is especially useful if you are working inside an ssh session and you need another terminal/pane** 

We will mostly work with terminal splits. 

There are huge changes in tmux from version to version. I will try to stick to the latest version. 




## Tricks: 

### As you try to copy from one pane, things from other pane is also selected?
With tmux 1.8+, do `prefix-z` too zoom a specific pane, copy it, then do `prefix-z` to come back to original panes. 
https://news.ycombinator.com/item?id=7758368 and https://github.com/tmux/tmux/issues/2096

### Copy paste
Copy:
* Press `prefix + [` to enter copy mode.
* Use arrow keys to go to the start/end of text selection.
* Press `ctrl + space` (If you have set ctrl + space as prefix, Press `ctrl + space + space` instead)
* Use arrow keys to move to the other side of selection. (See the color change)
* Press `ctrl + w`.
Paste:
* Press `prefix + ]` in insert mode.

[source](https://unix.stackexchange.com/a/333294)

### Scroll
* Press `prefix + [` to enter copy mode.
* scroll with arrow keys (or ...)
* Press `q` to exit. 
[source](https://superuser.com/questions/209437/how-do-i-scroll-in-tmux)
