# VS Code Tips/Shortcuts

Tips for using [Visual Studio Code](https://code.visualstudio.com/) editor/IDE.

## Keyboard Shortcuts
*[Cheat Sheet](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)* [pdf]  

| Shortcut Key | Action |
| :----------- | :----- |
| <kbd>Alt</kbd> + ↑ / ↓ | Move current line up/down. |
| <kbd>Ctrl</kbd> + / | Toggle comment (line or selection). |
| <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + \ | Jump to matching bracket. |
| <kbd>Ctrl</kbd> + ] / \[ | Indent/outdent line. |
| <kbd>Ctrl</kbd> + B | Toggle sidebar display. |
| <kbd>Ctrl</kbd> + C | Copy current line (empty selected). |
| <kbd>Ctrl</kbd> + P | Quick search for recently opened files by name. |
| <kbd>Ctrl</kbd> + X | Cut current line (empty selection). |
| <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + K | Delete line. |

## Recommended Settings
Probably the best feature of VS Code is its fantastic configurability. To access the **User** `settings.json` file, which contains the VS Code configuration parameters, File → Preferences → Settings or <kbd>Ctrl</kbd> + <kbd>,</kbd>. As the file name indicates, settings are stored as regular JSON key/value pairs. Here are some of the settings that I find most useful.

| Setting | Description |
| :------ | :---------- |
| `"files.trimFinalNewlines": true` | Removes blank lines from end of files. |
| `"files.trimTrailingWhitespace": true` | Removes extra whitespace from ends of lines. |
| `"editor.renderWhitespace": "all"` | Displays all whitespace characters (tabs, spaces, new lines, etc.) on screen. |
| `"editor.cursorStyle": "block"` | Displays cursor as a block. |
| `"editor.cursorBlinking": "smooth"` | Flashes cursor smoothly. |
| `"workbench.editor.highlightModifiedTabs": true` | Adds highlight to top of tab of unsaved editors for easy identification. |
| `"editor.minimap.enabled": false` | Disables the editor minimap. |
| `"editor.matchBrackets": "near"` | Matches bracket pairs only in close proximity. |
| `"workbench.sideBar.location": "right"` | Moves the side bar to the right side of VS Code window. |
| `"window.title": "${dirty}${activeEditorLong}"` | Displays full path and file name of current editor in VS Code title bar. |


## Favorite Extensions
- [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)
- [GitHub Plus Theme](https://github.com/thenikso/github-plus-theme)
- [VSCodeVim](https://github.com/VSCodeVim/Vim) - Vim emulator for VSCode