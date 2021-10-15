# Sway

Sway is a very minimal i3 like compositor. Simply, it's a window manager like openbox. But it's tiling, i.e., it will arrange the windows like a tile. 
An advantage is that, it is written in secure and efficient wayland rather than X11. 

**Seems too hard, but you want to try this lightweight windoe manager anyway? Just hop on!**

### Install (from an existing desktop manager):
If you have an existing account or desktop manager, first search about *sway* in your package manager and install it. 
Usually, your package manager may give optional dependency to choose for various other essential things like *Application launcher*, *status line*, *terminal*, *screen locker*, *customizable bar*, *screen locker*, *notification area* and so on,. In manjaro, using pacman, I got the following optional dependencies:


However, there are other options. Look here: https://github.com/swaywm/sway/wiki/Useful-add-ons-for-sway


Now usually, when you will log in for the next time, in some right-bottom corner, you can adjust your window manager to *sway*. 



### Usual Shortcuts:
First log in to sway might stumble you as there is nothing much you can see - it's minimalistic. So, it's good to know some shortcuts, especially for the application launcher, terminal, and exiting. https://www.reddit.com/r/swaywm/comments/he9imx/what_are_the_keyboard_shortcuts_for_sway/

However, you can change most of the keybindings later. 




Extra Keys:

``` ~/.config/sway/config
bindsym XF86AudioRaiseVolume exec pactl set-sink-volume @DEFAULT_SINK@ +5%
bindsym XF86AudioLowerVolume exec pactl set-sink-volume @DEFAULT_SINK@ -5%
bindsym XF86AudioMute exec pactl set-sink-mute @DEFAULT_SINK@ toggle
bindsym XF86AudioMicMute exec pactl set-source-mute @DEFAULT_SOURCE@ toggle
bindsym XF86MonBrightnessDown exec brightnessctl set 5%-
bindsym XF86MonBrightnessUp exec brightnessctl set +5%
bindsym XF86AudioPlay exec playerctl play-pause
bindsym XF86AudioNext exec playerctl next
bindsym XF86AudioPrev exec playerctl previous
bindsym XF86Search exec bemenu-run
```

