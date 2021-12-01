# Tmux Tips

Tips for effectively using [Tmux](https://github.com/tmux/tmux/wiki) terminal multiplexer.

## Basic Concepts

### Why not use tabbed terminals?
Linux has many fantastic tabbed terminal applications for the various GUI desktop environments. (My personal favorite is still the venerable minimalist [Xfce Terminal](https://docs.xfce.org/apps/xfce4-terminal/start), regardless of the DE that I'm using.) While these tools are great, if you just want to switch between shell environments, it's still difficult to avoid using the mouse.

Moreover, if you do any work with Linux servers remotely, such as via SSH, you probably don't have the luxury of a GUI environment on them. A similar scenario is with using the amazing Windows Subsystem for Linux (WSL) on Windows. It gives us a mostly complete Linux machine, but without a GUI, so we are somewhat restricted in how we can interact with it. And although tools like Windows Terminal are helpful, they get us back to the tabbed interface conundrum.

Tmux is tailormade for these use cases. It's a keyboard-centric tool that provides the convenience of multiple terminal sessions/instances with very low overhead, both in terms of system load and user cognitive load! Likewise, Tmux supports both Emacs and Vim keybindings, so many users don't even need to learn new keyboard shortcuts.

### Session - Window - Pane
Tmux is built around the basic paradigm of **sessions**, **windows**, and **panes**. Fundamentally, windows and panes are entirely optional; you can harness most of the of the power of Tmux directly from sessions.

**Sessions** provide a "container" for the shell instance/environment. Tmux manages these sessions and allows you to only keep open/active the sessions that you are currently using/working in. The others simply continue running in the background (hidden) waiting for you to call them up when needed. When working remotely, such as via SSH, Tmux provides an extra level of "protection" of your work environment in case you lose connectivity of your SSH session. You can simply reconnect, launch Tmux again on the remote machine and re-attach to the session!

**Windows** and **panes** provide a way to logically subdivide a session so that you can have multiple (independent) terminals displayed simultaneously. Think of **windows** like tabs in a web browser and **panes** as frames within a browser tab. For example, you can `tail` a log file in one window or pane, while reading a `man` page in another, and edit a file in Vim in a third.

When you create a new **window** (<kbd>Ctrl</kbd>-b + `c` _or_ `tmux new-window`), each new window is listed by index (0-9) on the tmux status bar with the active window indicated with an asterisk (`*`). By default, the new window will be active. To switch the active window, use <kbd>Ctrl</kbd>-b + `0-9` index.

## Common Commands
| Command      | Action |
| :----------- | :----- |
| `tmux` | Start new tmux session |
| `tmux ls` | List all running sessions with number of windows in each |
| `tmux a[ttach] -t 0` | Re-attach to session `0`. (If `-t 0` is **not** specified, it will attach to _last used/active_ tmux session.) |
| `tmux new -s name` | Create a new session called `name` |
| `tmux rename-session -t 0 name` | Rename session `0` to `name` |
| `tmux kill-session -t 0` | Terminate session `0` |
| `tmux list-keys` | Lists all bound keys and associated tmux commands |
| `tmux list-commands` | Lists all tmux commands and arguments/parameters |
| `tmux info` | Lists all current tmux sessions, windows, panes, etc. and their pids, etc. |
| `tmux source-file ~/.tmux.conf` | Reloads the tmux configuration specified in `~/.tmux.conf` |
| `tmux select-pane -t :.+` | Selects the next **pane** in numerical order |

## Common Keyboard Shortcuts
All commands initiated with **prefix** key combination, 'C-b' (<kbd>Ctrl</kbd>-b) by default.

| Shortcut Key | Command | Action |
| :----------- | :------ | :----- |
| `?` | `list-keys` | Help: Display brief shortcut key help |
| `:` | | Open tmux prompt in status bar |
| `[` | `copy-mode` | Opens Copy mode |
| `s` | `list-sessions` | List _sessions_ |
| `d` | `detach` | "Detach" from current _session_ (hide the session) |
| `$` | `rename-session` | Rename (or name) current _session_ |
| `t` | `clock-mode` | Display a clock in current window/pane |
| `c` | `new-window` | Create a new **window** |
| `,` | `rename-window` | Rename (or name) the current **window** |
| `&` | `kill-window` | Terminate the current **window** |
| `0` - `9` | `select-window -t :0-9` | Select **window** by index |
| `w` | `list-windows` | Show menu of windows and interactively select **window** |
| `n` | `next-window` | Switch to the _next_ **window** |
| `p` | `previous-window` | Switch to the _previous_ **window** |
| | `move-window -t {session_name}:` | Move current **window** to session named `session_name` |
| | `move-window -t n` | Move current **window** to index `n` in this session (assumes window with index `n` does _not_ exist) |
| | `swap-window -t n` | Swap current **window** with window with index `n` |
| `"` | `split-window [-v]` | Split current window/pane _vertically_ (top and bottom) |
| `%` | `split-window -h` | Split current window/pane _horizontally_ (left and right) |
| `↑` / `↓` / `←` / `→` | `select-pane -[UDLR]` | Move between panes |
| `{` or `}` | `swap-pane -[UDLR]` | Swap _contents_ (not index) of panes |
| <kbd>Ctrl</kbd> + `↑` / `↓` / `←` / `→` | `resize-pane -[UDLR] n` | Resize **pane** in direction of arrow by `n` cells |
| `!` | `break-pane` | Move the current **pane** into new separate **window** |
| | `join-pane -t n` | Move the current **window** or **pane** to a new **pane** in **window** index `n` |
| `q` | `display-panes` | Show pane indexes; type desired index to go to that pane. |
| `o` or `;` | | Toggle between current and previous **pane** |
| <kbd>Alt</kbd> + `1` - `5` | | Set pane layout to one of [five presets](https://github.com/tmux/tmux/wiki/Getting-Started#window-layouts): even-horizontal, even-vertical, main-horizontal, main-vertical, or tiled |
| <kbd>Space</kbd> | | Switch to next "standard" window/pane layout |
| <kbd>Ctrl</kbd> + `o` or <kbd>Alt</kbd> + `o` | `rotate` | Rotate _contents_ (not index) of panes _forwards_ or _backwards_ |
| `x` | `kill-pane` | Terminate the current pane |

## Customizing the `tmux.conf` configuration file
The default settings for Tmux are defined in the application itself, but you can see the global (all users) defaults by running:
```bash
tmux show -g
```
Likewise, the Tmux documentation installed with the application usually contains an example configuration file. On Ubuntu/Debian distributions, run `dpkg -L tmux` to get a list of the files installed and look for something like `example_tmux.conf`.

Just like all of the Linux "power tools", Tmux allows amazing customization via the `tmux.conf` configuration file. The default configuration file (on Linux) is `~/.tmux.conf`, but on many Linux distributions, such as Ubuntu, you must create this file.

### A good `tmux.conf`

### Test Driving Changes
However, before you make these changes, you might want to "test drive" some of these. You can do that by entering any of these commands interactively at the `tmux` command prompt (<kbd>Ctrl</kbd>-b :). Remember that, if the commands involve changing shortcut keys (a.k.a. keybindings), ensure that you have an `unbind` for each of your `bind` commands or you'll leave the old shortcut active, as well.

For example, to change the prefix key from <kbd>Ctrl</kbd>-b to <kbd>Ctrl</kbd>-a, since the `a` key is closer to the _left_ <kbd>Ctrl</kbd> key, you can do the following:
```
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix
```

## Using Tmux Copy Mode
Frequently, you'll need to copy and paste text between windows or panes in Tmux. Tmux has a built-in Copy mode just for this purpose. The basic _workflow_ for Copy mode is:
1. `prefix` + `[` - Start copy mode. Tmux will show the cursor position in the upper right corner of the active pane.
2. Move to the start of the text to copy.
3. <kbd>Ctrl</kbd> + <kbd>Space</kbd> - Start highlighting text to copy. Use regular keyboard navigation to extend the highlighted selection.
4. <kbd>Alt</kbd> + `w` - Copy the selection to the clipboard.
5. Navigate to desired target window/pane using normal Tmux commands/keyboard shortcuts.
6. `prefix` + `]` - Paste clipboard contents at cursor position in target window/pane.

This workflow works fine for copying and pasting from/to Vim editor instances running inside Tmux windows/panes.

## References
[Tmux Guide](https://tmuxguide.readthedocs.io/en/latest/tmux/tmux.html)  
[A tmux Crash Course](https://thoughtbot.com/blog/a-tmux-crash-course)  
[The Tao of tmux](https://tmuxp.git-pull.com/about_tmux.html)  
[Tmux Linux man page](https://man7.org/linux/man-pages/man1/tmux.1.html)  
[Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)