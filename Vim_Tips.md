# Vim Tips/Shortcuts

Tips for using [Vim](https://code.visualstudio.com/) editor.

## Basic Navigation/Movements
| Keystrokes | Command | Action |
| :--------- | :------ | :----- |
| <kbd>G</kbd> | **G** | Go to end of file/buffer |
| <kbd>gg</kbd> | **gg** | Go to start of file/buffer |
| 

## Basic Editing Movements
| Keystrokes | Command | Action |
| :--------- | :------ | :----- |
| <kbd>x</kbd> | **x** | **d**elete character under cursor |
| <kbd>i</kbd>/<kbd>a</kbd> | **i** / **a** | **i**nsert _at_ or **a**ppend _after_ cursor |
| <kbd>yy</kbd> | **yy** | duplicate current line (**yank yank**) |
| <kbd>y</kbd> | **y** | copy (**yank**) selection to clipboard |
| <kbd>p</kbd> | **p** | **p**aste clipboard _after_ cursor |

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
| `:b`_#_ | | Open buffer number _#_ (see `ls` below) |
| `:bp[revious]` | | Previous buffer |
| `:bd[elete]` | | Close current buffer |
| `:ls` | | List open buffers
```bash
:ls
  1 #    "space_rocks/__main__.py"      line 5
  2 %a   "space_rocks/game.py"          line 74
  3      "space_rocks/utils.py"         line 0
  4      "space_rocks/models.py"        line 0
```
 |

[Reference](https://www.tecmint.com/split-vim-screen/)  

## Indent/Outdent Multiple Lines
The quickest way to indent/outdent multiple lines is to enter **VISUAL** mode (press <kbd>v</kbd>), then select the desired rows using standard navigation (<kbd>j</kbd> and <kbd>k</kbd>). Then use <kbd>></kbd> to _indent_ and <kbd><</kbd> to _outdent_ the selected text.

[Reference](https://stackoverflow.com/a/7452318)  

## Comment/Uncomment Multiple Lines
Use [mark](http://vimdoc.sourceforge.net/htmldoc/motion.html#mark) to specify the range of lines to comment. In this example, we use the letter `t`.

Mark the start of the range of the lines to `mt`. Use arrow keys or `j` and `k` (or any navigation keys) to select the desired range. Then enter the command `:'t,.s/^/#/` to comment the select lines. This command means:
- `:` prefix for complex commands
- `'t,.` range for the command; in this case, from the marker `'t` to the current line `.`
- `s/^/#/` substitution command; in this case, replaces all beginning of line `^` with literal character `#`

To remove the `#` from the beginning of the lines, repeat the process with substitution command `s/^#//`.

[Reference](https://unix.stackexchange.com/a/120619)

## Check Color Support in Your Vim Instance
To determine the [color name values](https://vim.fandom.com/wiki/Xterm256_color_names_for_console_Vim) supported in your Vim instance, execute this command in Vim:
```
:run colortest.vim
```
This will open a new buffer with the color names and examples of these colors.