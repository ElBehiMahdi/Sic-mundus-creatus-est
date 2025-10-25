Tmux is a terminal multiplexer; it allows you to create several "pseudo terminals" from a single terminal

usefull for when im openning a terminal from my laptop 
and accessing my desktop to ssh i can access the terminal i openned from the laptop 

manage multiple terminals on your local machine

To start using tmux, type tmux on your terminal. This command launches a tmux server, creates a default session (number 0) with a single window, and attaches to it.


-htop on a terminal tmux 0
-a new ssh tab and typed tmux ls 
0: 1 windows (created Sat Oct 25 13:08:01 2025) (attached)

- tmux attach -t 0 to open the htop terminal from another ssh tab 
- manage terminals using Ctrl+B wait D to detach from it 
- check cheatsheet 
- Ctrl+B % ---> do a vertical split to switch between panes Ctrl+B O

