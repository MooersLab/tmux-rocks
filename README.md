![Version](https://img.shields.io/static/v1?label=tmux-rocks&message=0.3&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# tmux is easier than it looks

Tmux is a terminal mutliplexer.
It supports easy navigation between windows and splits a window into multiple panes.
It supports the foolish pursuit of multi-tasking.
Your multipaned terminal window gives on-lookers the illusion that you know something about computers.
A very attractive feature is that it is highly programmable using Bash.

I have been aware of `tmux` since my Vim year before switching to Emacs several years ago. 
`tmux` looked confusing to me at the time.
It no longer is because Emacs mimics multiplexing with its support for easy splitting of buffers.
As a result, I might have been better prepared to pick it up quite quickly.


<img width="1711" alt="Screenshot 2024-11-12 at 4 42 51 AM" src="https://github.com/user-attachments/assets/c1bda3ed-2f25-4d84-9256-704d519afc79">

I can see how splitting a terminal horizontally and vertically into several panes can be very useful when working on a remote computer.
In one pane, you could monitor running jobs, while in another, you could edit a script file to prepare the next job or an unrelated job.

Watch this 3-minute video [video](https://www.youtube.com/watch?v=vtB1J_zCv8I) to understand what is possible.
The pace is adequate to get a reasonably complete picture of what is possible
I used Copilot to explore the basics.
I made this cheat sheet after learning about its features because I want to use it in the future.

## Editor integration

The terminal-based text editors nano, Neovim, and terminal (emacs -nw) GNU Emacs have packages that support moving from the editor in one pane to other panes in a tmux window.

## Requirements

- git
- package manager like homebrew



## Installation

0. If tmux is not installed already, install it with a package manager. It is available in homebrew and Macports on macOS.
1. Download the tmux.conf file.
2. Store in the home directory as a hidden file: `~/.tmux.conf`
3. Install the tmux plugin manager: `git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`
4. `brew install reattach-to-user-namespace`
5. Activate it by entering `tmux source-file ~/.tmux.conf`.
6. Enter `tmux`  to start.
7. Enter `C-z c` several times to create new windows. Note the numbered windows in the status bar at the bottom. You can click on a number to switch to the corresponding window.

## The hierarchy: sessions, windows, and panes.

1. *Sessions* are at the same level as a terminal session. Each tab in iterm2 could be mapped to a different session. 
2. A *Window* corresponds to a terminal window or a single tab. Windows are numbered and can be renamed. Your single tab in iterm2 can be split into ten tmux windows.
3. A *pane* corresponds to the panes of a split window. The numbering of panes starts at 0; this too can be reset. Panes can also be named. The terminal sessions in each pane are independent.

The coordinates of a pane are <session-name>:#.# 
The first pound sign is the number of the window. 
This number can be substituted with the name of the window.
The second pound sign is the number of the pane.
Panes are only referred to by their number in this coordinate system.

## Cheatsheet

Start a tmux session by entering `tmux` at the prompt in the terminal.

The prefix key, by default, is `Crtl + b`. 
This is not Emacs friendly.
I remapped this `Cntrl-C` (or `C-z` in Emacs shorthand) in the configuration above because I rarely use it in Emacs.

You enter `Ctrl + z c` to open up a new tmux window.
The windows are numbered 0 onward.
The numbered sessions are shown in a panel at the bottom of the terminal.

To navigate to a specific terminal, enter `Ctrl + z` and then the terminal number.
To navigate to the next terminal, enter `Ctrl + z` and `n`.
To navigate to the previous terminal, enter `Ctrl + z` and `p`.

The above configuration enables the Shift key + right or left arrow to move between sessions.

A given terminal window can be split into panels.
Each panel is like an independent terminal session but not numbered.
To split the terminal vertically, enter `Ctrl + z` and then `%`.
To split the terminal horizontally, enter `Ctrl + z` and then `"`.

To kill a pane, enter `Ctrl + z` and then `x`. 
To kill a window, enter `Ctrl + z` and then `&`. 

To rename a window, enter `Ctrl + z` and comma.
To rename a pane, enter `printf '\033]2;%s\033\\' "My Pane"`
I made this into a bash function that takes the pane name as an argument.
This bash function works (easy-peasy):

```bash
function namePane {
echo "Name a tmux pane. Takes the name as a string in double quotes."
if [ $# -lt 1 ]; then
  echo 1>&2 "$0: not enough arguments"
  echo 'Usage1: namePane "My Pane"'
  return 2
elif [ $# -gt 1 ]; then
  echo 1>&2 "$0: too many arguments"
  echo 'Usage1: namePane "My Pane"'
  return 2
fi
printf '\033]2;%s\033\\' $1
}
```

## Session management

`tmux ls` will list running sessions.
`tmux attach` will bring a session to the foreground.
`tmux attach-session <session-name>` will attach a specific session. 
`tmux kill-servers` will kill all sessions.

## Enable Mouse Mode to click and drag the pane borders to resize them
Add the following line to your `~/.tmux.conf` file to enable mouse support:

```bash
set-option -g mouse on
```

Reload the tmux configuration with:
```bash
tmux source-file ~/.tmux.conf
```
This works very well!

## Commands in ~/.tmux.conf to resize the pane without using the mouse

Enter Alt (= Meta) and the arrow key repeatedly to change the size of the current pane.

```bash
bind -n M-Left resize-pane -L 5
bind -n M-Right resize-pane -R 5
bind -n M-Up resize-pane -U 5
bind -n M-Down resize-pane -D 5
```

## Command in ~/.tmux.conf to ease reloading the .tmux.conf file

Add the following:

```bash
# rebind r to ease reloading the config file after editing it
unbind r
bind r source-file ~/.tmux.conf
```

Enter `C-z r` to reload the `~/.tmux.conf` file after editing it.

If you have https://ohmyz.sh/ installed on macOS with the autocomplete plugin, the autocompletions work in tmux.
Hit the right arrow to accept an autocompletion in the terminal.
This makes navigation in the terminal faster than using the Finder.

## Getting help

1. Enter `C-z ?` to get a list of current keybindings.
2. Enter 'man tmux` to read the manual.
3. There are books about tmux, but this cheat sheet is a faster way to get started.

Your computer use will be transformed in an hour.
There will be no going back to a plain old terminal.

## More advanced config
![10windowsWeather](https://github.com/user-attachments/assets/adca220d-cf5f-4399-bf3c-526673ac6d7e)

The above image shows the application of a popular thematic tmux plugin, catppuccin.
The status bar had been moved to the top of the window.
The ten xtmux tabs (one per tmux window) iniside one iterm2 tab are for ten tmux windows opened in one session.
The highlighted tab corresponds to the current window.
The four-digit numbers are at the heart of my project management system.


The percentage by the heart is the charge of my failing battery.
I am not sure what `offline` refers to.
The temperature is the outside ambient air temperature.
Presumably, the location updates as you move from city to city.

Ten windows in an iterm2 tab is the limit.
The numbering scheme is the default that starts from 0.
This can be changed to start at one.
Starting at one is more ergonomic.
A bash script mentioned below automated the creation of the windows.

The thematic plugins are complex and do not always work as expected.
For example, the `advanced-tmux.conf` (rename .tmux.conf) file is configured to display the session name in the left end of the status bar.
It requires the installation of the tmux-plugin-manager.
Instead, we get a smiley face.
I thought I had installed all the Nerd Font files; maybe I overlooked several files of fonts.
Enter `C-z r` to get a brief display of the session name while reloading the configuration file.

## Runaway sessions
Some configs persist despite edits to the `tmux.conf` file and its reloading.
You probably have a runaway session.
Enter this command to kill all tmux servers: `tmux kill-server` and try again with a clean slate.

## Moving between tmux panes and text editor panes

Nona, vi, vim, neovim, and emacs (-nw option) run in the terminal.
Plugins on the tmux side and the editor side are required to switch seamlessly between the tmux and editor panes.
For example, the Emacs [tmux-pane package](https://github.com/laishulu/emacs-tmux-pane) enables jumping from the Emacs session in the top tmux pane below to the remote session on the schooner supercomputer in the tmux pane in the figure below by entering C-j.

![EmacsSchooner](https://github.com/user-attachments/assets/73035254-ef05-49af-8903-27c74c579d78)

## bashed-tmux  
The repo [bashed-tmux](https://github.com/MooersLab/tmux-bashed) has a bash script demonstrating how to automate the launching of about twenty customized tmux panes in five tabs of iterm2 and the opening of several files, applications, and a webpage.
This can ease the start of your daily routine after you have customized this template file.


|Version       |Changes                                                                                               |Date                  |
|:-------------|:-----------------------------------------------------------------------------------------------------|:--------------------:|
| Version 0.1  | Initiate project. Added badges, funding, and update table.                                           | 2024 November 12    |
| Version 0.2  | Added tmux.conf. Changed the prefix key binding.                                                      | 2024 November 13    |
| Version 0.3  | Added advanced-tmux.conf. Added blurb about rebinding r to ease reloading the config file. Added advanced image. | 2024 November 19    |

## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)



