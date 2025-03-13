# Installing Waybar
installing waybar
```
sudo pacman -S waybar
```

# Making Shortcuts
my shortcuts
```
bind = $mainMod, B, exec, google-chrome-stable
bind = $mainMod, C, exec, code
bind = $mainMod, E, exec, $fileManager
bind = $mainMod, F, exec, $browser
#bind = $mainMod, M, exit
bind = $mainMod, O, exec, obsidian
bind = $mainMod, Q, killactive,
bind = $mainMod, R, exec, $menu
# S is already for launching Special Space
bind = $mainMod, T, exec, $terminal
bind = $mainMod, V, exec, subl
bind = $mainMod, W, togglefloating
```
I removed `Win+M` as it is annoying rather we can create a wlogout and swaylock based lockscreen and we will use swayidle to automatically lock if no activity

# Installing wlogout
