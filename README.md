![Version](https://img.shields.io/static/v1?label=tmux-rocks&message=0.2&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# tmux is easier than it looks

Tmux is a terminal emulator on steroids.
It is good for impressing your colleagues.

<img width="1711" alt="Screenshot 2024-11-12 at 4 42 51 AM" src="https://github.com/user-attachments/assets/c1bda3ed-2f25-4d84-9256-704d519afc79">

Although iterm2 is highly customizable, tmux is a step beyond what you can do with iterm2.
It enables you to create multiple panes in a single terminal tab.
It also supports easy navigation between multiple terminal tabs.

I think that a very attractive feature is that it is highly programmable using Bash.

I can see how being able to split a terminal horizontally and vertically into several panes can be very useful when carrying out work on a remote computer.
In one pane you could be monitoring running jobs while another pane you could be editing a script file to prepare the next job or an unrelated job.

Watch this 3-minutes video (https://www.youtube.com/watch?v=vtB1J_zCv8I)[video] to get a sense of what is possible.
The pace is adequate to get a fairly full picture of what is possible
I used Copilot to explore the basics.
I made this cheat sheet as I learned about its features because I want to use it a lot in the future.

Start a tmux session by entering tmux at the prompt in the terminal.
You enter Ctrl + B to open up a new tmux window.
The windows are numbered 0 onward.
The numbered sessions are shown in a panel at the bottom of the terminal.

To navigate to a specific terminal, enter Ctrl + B and then the number of the terminal.
To navigate to the next terminal, enter Ctrl + B and N.
To navigate to the previous terminal, enter Ctrl + B and P.

A given terminal window can be split into panels.
Each panel is like an independent terminal session, but it is not numbered.
To split the terminal vertically, enter Ctrl + B and then %.
To split the terminal horizontally, enter Ctrl + B and then ".

To kill a pane, enter Ctrl + B and then X. 
To kill a window, enter Ctrl + B and then &. 

To rename a window, enter Ctrl +B and comma.
To rename a pane, enter    printf '\033]2;%s\033\\' "My Pane"
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


Enable Mouse Mode to click and drag the pane borders to resize them:
Add the following line to your ~/.tmux.conf file to enable mouse support:
```bash
set-option -g mouse on
```

Reload the tmux configuration with:
tmux source-file ~/.tmux.conf

This works very well!

Add these commands to ~/.tmux.conf to resize the pane without using the mouse.
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


## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)



