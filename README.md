![Version](https://img.shields.io/static/v1?label=tmux-rocks&message=0.3&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# tmux is easier than it looks

Tmux is a terminal multiplexer.
(If you use iterm2, it too supports terminal multiplexing. See [Mac OS Terminal Tips with iTerm2](https://www.youtube.com/watch?v=LcelO7XErdA). Regardless, this six-minute video is worth watching; it also revealed a number of bash functions that I did not know about.)
Tmux supports easy navigation between windows and splits a window into multiple panes.
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
The pace is adequate to get a reasonably complete picture of what is possible.
I used Copilot to explore the basics.
I made this cheat sheet after learning about the features of this plugin because I want to use it in the future.

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
The first pound sign is the index number of the window. 
This number can be substituted with the name of the window.
The second pound sign is the index number of the pane.
Only panes are referred to by their number in this coordinate system.

## Cheatsheet

Start a tmux session by entering `tmux` at the prompt in the terminal.

The prefix key, by default, is `Crtl + b`. 
This is not Emacs friendly.
I remapped this `Contrl-z` (or `C-z` in Emacs shorthand) in the configuration above because I rarely use `C-z` in Emacs.

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
The ten tmux tabs (one per tmux window) inside one iterm2 tab represent ten tmux windows opened in one session.
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
It requires the installation of the t`mux-plugin-manager`.
Instead, we get a smiley face.
I thought I had installed all the Nerd Font files.
Enter `C-z r` to get a brief display of the session name while reloading the configuration file.

## Runaway sessions
Some configs persist despite edits to the `tmux.conf` file and its reloading.
You probably have a runaway session.
Enter this command to kill all tmux servers: `tmux kill-server` and try again with a clean slate.

## Moving between tmux panes and text editor panes

Nano, vi, Vim, Neovim, and Emacs (-nw option) run in the terminal.
Plugins on the tmux side and the editor side are required to switch seamlessly between a pane running a terminal session and a pane occupied by an editor session.
For example, the Emacs [tmux-pane package](https://github.com/laishulu/emacs-tmux-pane) enables jumping from the Emacs session in the top tmux pane below to the remote session on the schooner supercomputer in the tmux pane in the figure below by entering C + j (C-j in Emacs convention) or vice versa with C-k.

The boundary between the two panes in the image below is the thin gray line.
The top pane with the Emacs session was enlarged in height compared to the terminal session in the pane at the bottom.
The enlargement was made by clicking on the boundary line and dragging it downward with the mouse.

![EmacsSchooner](https://github.com/user-attachments/assets/73035254-ef05-49af-8903-27c74c579d78)

## bashed-tmux  
The repo [bashed-tmux](https://github.com/MooersLab/tmux-bashed) has a bash script demonstrating how to automate the launching of about twenty customized tmux panes in five tabs of iterm2 and the opening of several files, applications, and a webpage.
This can ease the start of your daily routine after you have customized this template file.

## Tab titles
The default tab title in iterm2 will be tmux. If you have ten iterm2 tabs open, this row of `tmux` is not helpful.
After pasting the following code in your .zshrc file, you can change the title of an iterm2 tab by entering `tt`. 

```zsh
DISABLE_AUTO_TITLE="true"
tt () {
    echo -e "\033];$@\007"
}
```

Now, the bash script mentioned above can be modified to name iTerm tabs.

## Coloring the iterm2 tabs

Coloring the iterm2 tab can also enhance your identification of the tab of interest.
The following code assigns a random color for the tab each time a new tab is opened.
Add this code to your zshrc file.

```zsh
# randomly color new tabs.
PRELINE="\r\033[A"

function random {
    echo -e "\033]6;1;bg;red;brightness;$((1 + $RANDOM % 255))\a"$PRELINE
    echo -e "\033]6;1;bg;green;brightness;$((1 + $RANDOM % 255))\a"$PRELINE
    echo -e "\033]6;1;bg;blue;brightness;$((1 + $RANDOM % 255))\a"$PRELINE
}

function color {
    case $1 in
    green)
    echo -e "\033]6;1;bg;red;brightness;57\a"$PRELINE
    echo -e "\033]6;1;bg;green;brightness;197\a"$PRELINE
    echo -e "\033]6;1;bg;blue;brightness;77\a"$PRELINE
    ;;
    red)
    echo -e "\033]6;1;bg;red;brightness;270\a"$PRELINE
    echo -e "\033]6;1;bg;green;brightness;60\a"$PRELINE
    echo -e "\033]6;1;bg;blue;brightness;83\a"$PRELINE
    ;;
    orange)
    echo -e "\033]6;1;bg;red;brightness;227\a"$PRELINE
    echo -e "\033]6;1;bg;green;brightness;143\a"$PRELINE
    echo -e "\033]6;1;bg;blue;brightness;10\a"$PRELINE
    ;;
    *)
    random
    esac
}

color
```

With `tt` and random coloring enabled, my iterm terminal appears in the figure below.
The tabs labeled zsh are not tmux sessions, or they were intended tmux sessions,
but a bug in my bash function failed to launch a tmux session in the iterm2 tab.

<img width="1363" alt="Screenshot 2024-11-30 at 8 12 46 AM" src="https://github.com/user-attachments/assets/569a5ba2-bc12-4d54-80bb-867f6b3fe76e">

## Update history
|Version       |Changes                                                                                               |Date                  |
|:-------------|:-----------------------------------------------------------------------------------------------------|:--------------------:|
| Version 0.1  | Initiate project. Added badges, funding, and update table.                                           | 2024 November 12    |
| Version 0.2  | Added tmux.conf. Changed the prefix key binding.                                                      | 2024 November 13    |
| Version 0.3  | Added advanced-tmux.conf. Added blurb about rebinding r to ease reloading the config file. Added advanced image. | 2024 November 19    |
| Version 0.4  | Added notes about multiplexing in iterm2, tab titles, and tab coloring.                                |2024 November 30   |
| Version 0.4  | Fixed some typos.                                                                                     |2025 February 25   |

## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)
