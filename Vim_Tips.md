# Vim Tips/Shortcuts

Tips for using [Vim](https://code.visualstudio.com/) editor.

## Find location of Vim configuration files
Vim stores configuration information in files variously named `.vimrc`, `vimrc`, `.gvimrc`, and `gvimrc`, depending on the OS/platform and version. The `g` applies to the GUI version of the Vim.  
To find the location of the system/global and user-specific version of the configuration files used by Vim, run the `:version` commmand. It will return something similar to (from Ubuntu):
```bash
   system vimrc file: "$VIM/vimrc"
     user vimrc file: "$HOME/.vimrc"
 2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "$HOME/.exrc"
  system gvimrc file: "$VIM/gvimrc"
    user gvimrc file: "$HOME/.gvimrc"
2nd user gvimrc file: "~/.vim/gvimrc"
       defaults file: "$VIMRUNTIME/defaults.vim"
    system menu file: "$VIMRUNTIME/menu.vim"
  fall-back for $VIM: "/usr/share/vim"
```
Likewise, to see the full path and file name of your _user_ configuration file, run `:echo $MYVIMRC`. If nothing is returned, that means you do not have one.  

[Reference](https://stackoverflow.com/questions/8977649/how-to-locate-the-vimrc-file-used-by-vim-editor)

## Managing windows (buffers) and splits

| Command | Shortcut Key | Action               |
| :------ | :----------- | :------------------- |
| `:sp[lit]` | <kbd>Ctrl</kbd> + W <kbd>s</kbd> | Horizontal split |
| `:vsp[lit]` | <kbd>Ctrl</kbd> + W <kbd>v</kbd> | Vertical split |
| | <kbd>Ctrl</kbd> + W <kbd>h</kbd> / <kbd>j</kbd> / <kbd>k</kbd> / <kbd>l</kbd> | Move to _left_ / _upper_ / _lower_ / _right_ split <br />(same as regular navigation key bindings) |
| | <kbd>Ctrl</kbd> + W <kbd><</kbd> / <kbd>></kbd> | Decrease / increase **width** of **vertical** split |
| | <kbd>Ctrl</kbd> + W <kbd>-</kbd> / <kbd>+</kbd> | Decrease / increase **height** of **horizontonal** split
| `:enew` | | Open new/empty buffer |
| `:bn[ext]` | | Next buffer |
| `:bp[revious]` | | Previous buffer |
| `:bd[elete]` | | Close current buffer |
| `:ls` | | List open buffers |

[Reference](https://www.tecmint.com/split-vim-screen/)  