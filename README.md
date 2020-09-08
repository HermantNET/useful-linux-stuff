### bspwm

##### Layouts

In the bspwm git repo there's an examples folder with a folder called receptacles. Basically what you need to do is
copy the files in the folder somewhere so you execute them, then run the following commands to copy your current layout.

Output the current layout to a file `bspc wm -d > ~/Documents/layout.json`

Gen the canvas/receptacles `python3 extract_canvas ~/Documents/layout.json > ~/Documents/canvas.json`

Get the rules for bspc `python3 induce_rules ~/Documents/layout.json`

Make a script and paste the rules + load the canvas

###### layout.sh
```
bspc wm -l /home/thomas/Documents/test-canvas.json

bspc rule -a "Info Terminal":"Info Terminal" -o node=@^1:^1:/1/1
bspc rule -a "Terminal Music":"Terminal Music" -o node=@^1:^1:/1/2/1
bspc rule -a Anki:anki -o node=@^1:^1:/1/2/2
bspc rule -a qutebrowser:qutebrowser -o node=@^1:^1:/2
bspc rule -a kitty:kitty -o node=@^1:^2:/
```

Finally source it at the end of your bspwmrc e.g. `source /home/thomas/bin/layout.sh`


##### Put focused window into preselect area

`bspc node -n last.!automatic.local`

##### Logout safely

Keybind or make an alias to the following script

###### logout.sh

```
#!/bin/bash

for window_id in $(bspc query -W); do
  bspc window $window_id -c
done

bspc quit

```

---

### Sxhkd

##### Useful keybinds

```
# Audio controls

XF86AudioRaiseVolume
	amixer -q -D pulse sset Master 5%+ unmute

XF86AudioLowerVolume
	amixer -q -D pulse sset Master 5%- unmute

XF86AudioMute
	amixer -q -D pulse sset Master toggle

```

---

### Rofi

`yay -S rofi`

##### Run bash scripts

Keybind this to something

`rofi -run-list-command ". ~/.config/rofi/get_aliases.sh" -run-command "/bin/bash -i -c '{cmd}'" -rnow`

###### get_aliases.sh

```
alias | awk -F'[ =]' '{print $2}'

```

---

### cron

`sudo pacman -S crond`

##### Execute scripts

Using notify-send as an example

`* * * * *  XDG_RUNTIME_DIR=/run/user/$(id -u) notify-send "wow" "very script"`

---

### .bashrc
##### omit "cd"

`shopt -s autocd`

---

### .inputrc
##### Autocomplete and case insensitive stuff

```
$include /etc/inputrc
set completion-ignore-case on
set show-all-if-ambiguous on
set visible-stats on
set show-all-if-unmodified on
set menu-complete-display-prefix on
"\t": menu-complete
"\e[Z": menu-complete-backward
```

---

### Autostart applications
##### dapper

`yay -S dapper`

Add `exec dapper -u &` to `~/.xinitrc`
Add applications you want to autostart to `~/.config/autostart/`
Example application reference

###### ~/.config/autostart/qutebrowser.desktop
```
[Desktop Entry]
Type=Application
Exec="/usr/bin/qutebrowser"
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name="qutebrowser"
```

---

### Kakoune
##### Copy to system clipboard

Make your selection, then press `< alt - | >`, from there enter `xsel -i -b`

##### Paste

If you're having weird issues when pasting it's probably because of hooks, to disable hooks and paste
in normal mode type `\` then enter insert mode `i` and paste normally with `< ctrl - shift - v >`

---

### Kitty
##### My terminal config

```
font_family VictorMono Nerd Font
bold_font auto
italic_font auto
bold_italic_font auto

font_size 11.0

window_padding_width 0 8

background_opacity 0.8

background #262626
foreground #ffcb83
cursor #fb521c
selection_background #c03f1f
color0 #000000
color8 #6a4e29
color1 #c03900
color9 #ff8b67
color2 #a3a900
color10 #f6ff3f
color3 #caae00
color11 #ffe36e
color4 #bd6c00
color12 #ffbd54
color5 #fb5d00
color13 #fc874f
color6 #f79400
color14 #c59752
color7 #ffc88a
color15 #f9f9fe
selection_foreground #262626
```

---

### Virt manager
##### Installing

https://gist.github.com/diffficult/cb8c385e646466b2a3ff129ddb886185
