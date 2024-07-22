# How to install a tiling window manager on mac using Yabai and skhd

To get started visit the <a href="https://github.com/koekeishiya/yabai" >Yabai GitHub page </a> and read the documentation to get familiarized as well as the <a href="https://github.com/koekeishiya/skhd"> skhd github page </a> which is a dependency used mainly for keyboard shortcuts. 

# Why?
Tiling window managers make life easier. You will never be stuck finding a small application hidden under 5 or more windows. With a tiling window manager, it will manage the windows for you so that they are automatically resized. A big philosophy that a lot of users have with tiling window mangers is to use multiple desktops or spaces and have mainly one application at a time for each space. With a tiling window manager like yabai, we can make our life easier and even do cool things like having keyboard shorcuts to move applications to a specific desktop or space. It also gives you +1 coding skills and cool factor(yes really). 
# Installation
**A lot of the information in this documentation was taken from  <a href="https://www.josean.com/posts/yabai-setup"> this blog post </a> .** 
To start you will want to do a couple changes in your settings 
## configure MacOs Specific Settings

1.  Open Several Desktops (~7) on Your Machine
2.  Go To Keyboard Settings > Shortcuts > Mission Control
3.  Expand Mission Control and Turn On Shortcuts for Switching Spaces 1-7 with “Ctrl + # Of Space”, for example "Ctrl+2" will bring me to desktop 2 or Space 2. (You can use other keyboard shortcuts, I personally use the option key for this) 
4.  Go to System Settings > Accessibility > Display
5.  Turn On Reduce Motion
6.  Go To System Settings > Desktop & Dock > Mission Control
7.  Turn off “Automatically Rearrange Spaces Based On Most Recent Use”
8.  Personally, I only keep “Displays Have Separate Spaces” turned on here, and that’s what I’d recommend

## Using Homebrew

### Install Yabai

Run the following command:

```
brew install koekeishiya/formulae/yabai
```

### Install Skhd

Run the following command:

```
brew install koekeishiya/formulae/skhd
```

## Create Yabai Config File in Home Directory
The file explorer is accessible using the button in left corner of the navigation bar. You can create a new file by clicking the **New file** button in the file explorer. You can also create folders by clicking the **New folder** button.

## Add Configuration Options to File

Open with preferred editor and add the following (“open -t yabairc”, “code yabairc” (visual studio code), “vim yabairc”, etc…). There are some configuration options that will be available only if you partially disable SIP (Sytem Integrity Protection). All of the options I’ve configured below will work without disabling SIP.

Configure default layout to use: Binary Space Partitioning.

```
# default layout (can be bsp, stack or float)
yabai -m config layout bsp
```

Configure how window splits should be made:

```
# New window spawns to the right if vertical split, or bottom if horizontal split
yabai -m config window_placement second_child
```

Configure window padding:

```
# padding set to 12px
yabai -m config top_padding 12
yabai -m config bottom_padding 12
yabai -m config left_padding 12
yabai -m config right_padding 12
yabai -m config window_gap 12
```

Configure mouse settings:

```
# center mouse on window with focus
yabai -m config mouse_follows_focus on

# modifier for clicking and dragging with mouse
yabai -m config mouse_modifier alt
# set modifier + left-click drag to move window
yabai -m config mouse_action1 move
# set modifier + right-click drag to resize window
yabai -m config mouse_action2 resize


# when window is dropped in center of another window, swap them (on edges it will split it)
yabai -m mouse_drop_action swap
```

Disable specific apps from being managed with yabai. Use this format for the apps you’d like to disable.
**I highly reccomend disabling microsfot teams classic because it has some weird behaviors for focusing the mouse**

```
yabai -m rule --add app="^System Settings$" manage=off
yabai -m rule --add app="^Calculator$" manage=off
yabai -m rule --add app="^Karabiner-Elements$" manage=off
yabai -m rule --add app="^Microsoft Teams classic" manage=off
```

### Start Yabai

Start it like so:

```
yabai --start-service
```

Allow any prompts for accessibility permissions.

Restart yabai:

```
yabai --restart-service
```

### Start Skhd

Start it like so:

```
skhd --start-service
```
Allow any prompts for accessibility permissions.

Restart skhd:

```
skhd --restart-service
```
# IMPORTANT! Enable Accessibility for yabai and skhd
In order for these programs to work and to automatically reisze your windows and move your mouse and focus on things you will need to have enabled these in the settings under accessibility. It will most likely prompt you for this when you attempt to run the programs and you can enable them with administrative access or permission.  

If not, open `System Settings.app` and navigate to `Privacy & Security`, then `Accessibility`. Click the + button at the bottom left of the list view and enter your password to allow changes to the list.

Starting with `yabai --start-service` will usually prompt the user to allow `yabai` accessibility permissions. Check the box next to `yabai` to allow accessibility permissions.
## Create Skhd Config File in Home Directory

Move to home folder:

```
cd ~
```

Create directory for skhd config file and move into it (config will live in “.config/skhd/skhdrc”):

```
mkdir .config/skhd
cd .config/skhd
```

Create skhd config file:

```
touch skhdrc
```

## Add Keyboard Shortcuts To Config File

These shortcuts are completely up to you, but you can use mine as reference. The way I like to do it is to choose two primary modifiers and a third if necessary. These could be something like “alt”, “shift + alt”, and “ctrl + alt”.

One recommendation is to change “shift + alt” to “hyper” if you have a “hyper” key on your keyboard (“command” + “control” + “shift” + “alt”). You could configure a “hyper” key (replacing “Caps Lock” for example) with something like “Karabiner-Elements”.

### Changing Focus Shortcuts

#### Change Focus Within Space
**You may notice that hjkl are the keys for west, south, north, and east. This is because I use vim keys instead of the arrow keys. The arrow keys are farther away from where the fingers usually rest on the keyboard so using hjkl has less unnecessary hand movements but you can tailor it your liking.**
```
# change window focus within space
alt - j : yabai -m window --focus south
alt - k : yabai -m window --focus north
alt - h : yabai -m window --focus west
alt - l : yabai -m window --focus east
```

#### Change Focus Between Displays

```
#change focus between external displays (left and right)
alt - s: yabai -m display --focus west
alt - g: yabai -m display --focus east
```

### Shortcuts For Modifying The Layout
**The alt key is also the option key for mac keyboards**

```
# rotate layout clockwise
shift + alt - r : yabai -m space --rotate 270

# flip along y-axis
shift + alt - y : yabai -m space --mirror y-axis

# flip along x-axis
shift + alt - x : yabai -m space --mirror x-axis

# toggle window float
shift + alt - t : yabai -m window --toggle float --grid 4:4:1:1:2:2
```

### Modifying Window Size Shortcuts

```
# maximize a window
shift + alt - m : yabai -m window --toggle zoom-fullscreen

# balance out tree of windows (resize to occupy same area)
shift + alt - e : yabai -m space --balance
```

### Shortcuts For Moving Windows Around

#### Swap Windows Within Space

```
# swap windows
shift + alt - j : yabai -m window --swap south
shift + alt - k : yabai -m window --swap north
shift + alt - h : yabai -m window --swap west
shift + alt - l : yabai -m window --swap east
```

#### Move Window Within Space And Split

```
# move window and split
ctrl + alt - j : yabai -m window --warp south
ctrl + alt - k : yabai -m window --warp north
ctrl + alt - h : yabai -m window --warp west
ctrl + alt - l : yabai -m window --warp east
```

#### Move Window Across Displays

```
# move window to display left and right
shift + alt - s : yabai -m window --display west; yabai -m display --focus west;
shift + alt - g : yabai -m window --display east; yabai -m display --focus east;
```

#### Move Window To A Space (Desktop/Workspace)

```
#move window to prev and next space
shift + alt - p : yabai -m window --space prev;
shift + alt - n : yabai -m window --space next;

# move window to space #
shift + alt - 1 : yabai -m window --space 1;
shift + alt - 2 : yabai -m window --space 2;
shift + alt - 3 : yabai -m window --space 3;
shift + alt - 4 : yabai -m window --space 4;
shift + alt - 5 : yabai -m window --space 5;
shift + alt - 6 : yabai -m window --space 6;
shift + alt - 7 : yabai -m window --space 7;
```

### Stop/Start/Restart Yabai

```
# stop/start/restart yabai

#this usually locks your computer so I reccomend using a different key combination
ctrl + alt - q : yabai --stop-service
ctrl + alt - s : yabai --start-service
ctrl + alt - r : yabai --restart-service
```

## That’s It, You’re Done!

## Learning More

If you’d like to explore everything you can do with yabai open the man page:

```
man yabai
```

--- or ---

[Yabai Github Repo](https://github.com/koekeishiya/yabai)

[Yabai Wiki](https://github.com/koekeishiya/yabai/wiki)

[Yabai Documentation](https://github.com/koekeishiya/yabai/blob/master/doc/yabai.asciidoc)

[Skhd Github Repo](https://github.com/koekeishiya/skhd)


