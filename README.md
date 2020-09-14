### pacman

##### Fix db locked

```
sudo rm /var/lib/pacman/db.lck
```

---

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

##### VM image location

```
/var/lib/libvirt/images/
```

##### Resizing a VM's storage

https://sandilands.info/sgordon/increasing-kvm-virtual-machine-disk-using-lvm-ext4

Don't forget the last step using resize2fs

--- 

### BlackArch
##### Enable Networking

If you installed BlackArch and networking/internet wasn't working, you can try

```
systemctl enable dhcpcd && systemctl start dhcpcd
```

If that causes issues with pacman you can disable and stop it, then try netctl instead.

If netctl is giving you an error when trying to start a profile, try this.

```
ip link set wlan0 down
netctl start profile
```

##### Masscan is broken!

If masscan doesn't work (gets stuck on "waiting to close"), build it from source and add it to `/usr/local/bin` (call it something like mscan).

---

### Spectrwm config

```
dialog_ratio = 0.6
border_width = 1
color_focus = rgb:62/98/e0
color_unfocus = rgb:31/6a/b7
window_name_enabled = 1
clock_enabled = 1
clock_format = |    %a %b %d %Y %T     |
bar_color = rgb:00/00/00
bar_border = rgb:62/98/e0
bar_border_width = 1
bar_font_color = rgb:aa/aa/aa
bar_font = xos4 Terminus:size=12:antialias=true
bar_at_bottom = 0
bar_delay = 1
#bar_action = battery.sh
stack_enabled = 1
verbose_layout = 1
workspace_limit = 10
focus_mode = default
focus_close = previous
focus_close_wrap = 1
focus_default = last
tile_gap = 0
region_padding = 0

program[term] = /usr/bin/urxvt
program[menu] = dmenu_run -nb $bar_color -nf $bar_font_color -sb $bar_border \
  -sf darkgray -fn "xos4 Terminus:size=12"

bind[term] = Mod4+Return
bind[wind_del] = Mod4+w
bind[float_toggle] = Mod4+t
bind[menu] = Mod4+space

bind[focus_next] = Mod4+j
bind[focus_prev] = Mod4+k

bind[height_grow] = Mod4+equal
bind[height_shrink] = Mod4+minus

bind[maximize_toggle] = Mod4+f
bind[swap_main] = Mod4+g
bind[swap_next] = Mod4+Shift+j
bind[swap_prev] = Mod4+Shift+k

bind[master_grow] = Mod4+l
bind[master_shrink] = Mod4+h

bind[ws_1] = Mod4+1
bind[ws_2] = Mod4+2
bind[ws_3] = Mod4+3
bind[ws_4] = Mod4+4
bind[ws_5] = Mod4+5
bind[ws_6] = Mod4+6
bind[ws_7] = Mod4+7
bind[ws_8] = Mod4+8
bind[ws_9] = Mod4+9
bind[ws_10] = Mod4+0

bind[mvws_1] = Mod4+Shift+1
bind[mvws_2] = Mod4+Shift+2
bind[mvws_3] = Mod4+Shift+3
bind[mvws_4] = Mod4+Shift+4
bind[mvws_5] = Mod4+Shift+5
bind[mvws_6] = Mod4+Shift+6
bind[mvws_7] = Mod4+Shift+7
bind[mvws_8] = Mod4+Shift+8
bind[mvws_9] = Mod4+Shift+9
bind[mvws_10] = Mod4+Shift+0

bind[ws_1] = Mod+F1
bind[ws_2] = Mod+F2
bind[ws_3] = Mod+F3
bind[ws_4] = Mod+F4
bind[ws_5] = Mod+F5
bind[ws_6] = Mod+F6
bind[ws_7] = Mod+F7
bind[ws_8] = Mod+F8
bind[ws_9] = Mod+F9
bind[ws_10] = Mod+F10

#autorun = ws[1]: xterm
```

---

### PulseAudio
##### Audio in one app is muted when another makes a sound

https://wiki.archlinux.org/index.php/PulseAudio/Troubleshooting#Starting_an_application_interrupts_other_app.27s_sound
