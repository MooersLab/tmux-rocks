![Version](https://img.shields.io/static/v1?label=tmux-rocks&message=0.2&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# tmux is easier than it looks

Tmux is a terminal mutliplexer.
It supports easier navigation between windows and splits a window into multiple panes.
It supports the foolish pursuit of multi-tasking.
Your multipaned terminal window gives on-lookers the illusion that you know something about computers.
A very attractive feature is that it is highly programmable using Bash.


<img width="1711" alt="Screenshot 2024-11-12 at 4 42 51 AM" src="https://github.com/user-attachments/assets/c1bda3ed-2f25-4d84-9256-704d519afc79">

I can see how splitting a terminal horizontally and vertically into several panes can be very useful when working on a remote computer.
In one pane, you could monitor running jobs, while in another, you could edit a script file to prepare the next job or an unrelated job.

Watch this 3-minute video [video](https://www.youtube.com/watch?v=vtB1J_zCv8I) to understand what is possible.
The pace is adequate to get a reasonably complete picture of what is possible
I used Copilot to explore the basics.
I made this cheat sheet after learning about its features because I want to use it in the future.

## Editor integration

The terminal-based editors nano, Neovim, and terminal (emacs -nw) GNU Emacs have packages that support moving from the editor in one pane to other panes in a tmux window.

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
2. A *Window* corresponds to a terminal window or a single tab. Windows are numbered and can be renamed.
3. A *pane* corresponds to the panes of a split window. Panes can also be named. The terminal sessions in each pane are independent.

## Cheatsheet

Start a tmux session by entering `tmux` at the prompt in the terminal.

The prefix key, by default, is Crtl + B. This is not Emacs friendly.
I remapped this C-z in the configuration above because I rarely use it in Emacs.

You enter Ctrl + z c to open up a new tmux window.
The windows are numbered 0 onward.
The numbered sessions are shown in a panel at the bottom of the terminal.

To navigate to a specific terminal, enter `Ctrl + z` and then the terminal number.
To navigate to the next terminal, enter `Ctrl + z` and `n`.
To navigate to the previous terminal, enter `Ctrl + z` and `p`.

The above configuration enables the Shift key + right or left arrow to move between sessions.

A given terminal window can be split into panels.
Each panel is like an independent terminal session but not numbered.
To split the terminal vertically, enter Ctrl + z and then %.
To split the terminal horizontally, enter Ctrl + z and then ".

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

`tmux ls` will list running sessions.
`tmux attach` will bring a session to the foreground.


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

## Commands to ~/.tmux.conf to resize the pane without using the mouse.

Enter Alt (= Meta) and the arrow key repeatedly to change the size of the current pane.

```bash
bind -n M-Left resize-pane -L 5
bind -n M-Right resize-pane -R 5
bind -n M-Up resize-pane -U 5
bind -n M-Down resize-pane -D 5
```


If you have https://ohmyz.sh/ installed on macOS with the autocomplete plugin, the autocompletions work in tmux.
Hit the right arrow to accept an autocompletion in the terminal.
This makes navigation in the terminal faster than using the Finder.

There are books about tmux, but this cheat sheet is a faster way to get started.
Your computer use will be transformed in an hour.
There will be no going back to a plain old terminal.

|Version       |Changes                                                                                               |Date                  |
|:-------------|:-----------------------------------------------------------------------------------------------------|:--------------------:|
| Version 0.1  | Initiate project. Added badges, funding, and update table.                                           | 2024 November 12    |
| Version 0.2  | Added tmux.conf. Change the prefix key binding.                                                      | 2024 November 13    |

## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)



