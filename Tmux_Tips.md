# Tmux Tips

Tips for effectively using [Tmux](https://github.com/tmux/tmux/wiki) terminal multiplexer.

## Basic Concepts

### Why not use tabbed terminals?
Linux has many fantastic tabbed terminal applications for the various GUI desktop environments. (My personal favorite is still the venerable minimalist [Xfce Terminal](https://docs.xfce.org/apps/xfce4-terminal/start), regardless of the DE that I'm using.) While these tools are great, if you just want to switch between shell environments, it's still difficult to avoid using the mouse.

Moreover, if you do any work with Linux servers remotely, such as via SSH, you probably don't have the luxury of a GUI environment on them. A similar scenario is with using the amazing Windows Subsystem for Linux (WSL) on Windows. It gives us a mostly complete Linux machine, but without a GUI, so we are somewhat restricted in how we can interact with it. And although tools like Windows Terminal are helpful, they get us back to the tabbed interface conundrum.

Tmux is tailormade for these use cases. It's a keyboard-centric tool that provides the convenience of multiple terminal sessions/instances with very low overhead, both in terms of system load and user cognitive load! Likewise, Tmux supports both Emacs and Vim keybindings, so many users don't even need to learn new keyboard shortcuts.

### Session - Window - Pane
Tmux is built around the basic paradigm of sessions, windows, and panes. Fundamentally, windows and panes are entirely optional; you can harness most of the of the power of Tmux directly from sessions.
Sessions provide a "container" for shell instance/environment. Tmux manages these sessions and allows you to only keep open/active the sessions that you are currently using/working in. The others simply continue running in the background (hidden) waiting for you to call them up when needed. When working remotely, such as via SSH, Tmux provides an extra level of "protection" of your work environment in case you lose connectivity of your SSH session. You can simply reconnect, launch Tmux again on the remote machine and re-attach to the session!
Windows and panes provide a way to logically subdivide a session so that you can have multiple (independent) terminals displayed simultaneously. For example, you can `tail` a log file in one pane, while reading a `man` page in another, and edit a file in Vim in a third.

## Common Commands
| Command      | Action |
| :----------- | :----- |
| `tmux` | Start new tmux session |
| `tmux ls` | List all running sessions with number of windows in each |
| `tmux a[ttach] -t 0` | Re-attach to session 0 |
| `tmux new -s name` | Create a new session called _name_ |
| `tmux rename-session -t 0 name` | Rename session 0 to _name_ |
| `tmux kill-session -t 0` | Terminate session 0 |




## Common Keyboard Shortcuts
All commands initiated with prefix key combination, 'C-b' (<kbd>Ctrl</kbd>-b) by default.

| Shortcut Key | Action |
| :----------- | :----- |
| ? | Help: Display brief shortcut key help |
| s | List _sessions_ |
| d | "Detach" from current _session_ (hide the session) |
| $ | Rename current _session_ |
| c | Create a new **window** |
| , | Rename the current **window** |
| & | Terminate the current **window** |
| 0 - 9 | Select **window** by index |
| w | Show menu of windows and interactively select **window** |
| n | Switch to the _next_ **window** |
| p | Switch to the _previous_ **window** |
| " | Split current window/pane _vertically_ (top and bottom) |
| % | Split current window/pane _horizontally_ (left and right) |
| ↑ / ↓ / ← / → | Move between panes |
| <kbd>Ctrl</kbd> + ↑ / ↓ / ← / → | Resize pane in direction of arrow |
| x | Terminate the current pane |

## Customizing the `tmux.conf` configuration file
Just like all of the Linux "power tools", Tmux allows amazing customization via the `tmux.conf` configuration file. 

### Test Driving Changes
However, before you make these changes, you might want to "test drive" some of these. You can do that by entering any of these commands interactively at the `tmux` command prompt (<kbd>Ctrl</kbd>-b :). Remember that, if the commands involve changing shortcut keys (a.k.a. keybindings), ensure that you have an `unbind` for each of your `bind` commands or you'll leave the old shortcut active, as well.

For example, to change the prefix key from <kbd>Ctrl</kbd>-b to <kbd>Ctrl</kbd>-a, since the `a` key is closer to the _left_ <kbd>Ctrl</kbd> key, you can do the following:
```
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix
```



## References
[Tmux Guide](https://tmuxguide.readthedocs.io/en/latest/tmux/tmux.html)