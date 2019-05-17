## Ctrl commands.
```
Ctrl+A or Home: Go to the beginning of the line.

Ctrl+B: Move backward one character.

Ctrl+D: Close the terminal. 

Ctrl+D or Delete: Delete the character under the cursor.

Ctrl+E or End: Go to the end of the line.

Ctrl+F: Move forward one character.

Ctrl+G: Leave history searching mode without running a command.

Ctrl+H or Backspace: Delete the character before the cursor.

Ctrl+J: Same as ENTER/RETURN key(Ctrl+M).

Ctrl+K: Cut all characters after the cursor and adding them to the clipboard.

Ctrl+L: Clear the screen. 

Ctrl+N or Down Arrow: Next command in history. 

Ctrl+O: Run a command you found with Ctrl+R.

Ctrl+P or Up Arrow: Previous command in history. 

Ctrl+Q: Resume output to the screen after stopping it with Ctrl+S.

Ctrl+R: Searches the history backward.

Ctrl+S: Stop all output to the screen. 

Ctrl+T: Swaps the last two characters.

Ctrl+U: Cut the part of the line before the cursor, adding it to the clipboard.

Ctrl+V: Makes the next character typed verbatim.

Ctrl+W: Cut the word before the cursor, adding it to the clipboard.

Ctrl+XX: Move between the beginning of the line and the current position of the cursor. 

Ctrl+Y: Paste the last deleted item(yank).

Ctrl+_: Undo your last key press. Can be repeated multiple times.

Ctrl+[: ESC key.
```
## Alt commands.
```
Alt+A: Go to the beginning of a line.

Alt+B: Go left one word.

Alt+C: Capitalize the character under the cursor. The cursor moves to the end of the current word.

Alt+D: Delete the word after the cursor.

Alt+F: Go right one word.

Alt+L: Uncapitalize every character from the cursor to the end of the current word.

Alt+R: Revert any changes to a command pulled from your history.

Alt+T: Swap the current word with the previous word.

Alt+U: Capitalize every character from the cursor to the end of the current word.

Alt+.: Use the last word of the previous command.
       For example: mkdir qqq; cd Alt+.

!! - Repeat the last command.
```

## Switch modes.
```
Vi mode:
set -o vi 

Emacs mode:
set -o emacs 
```
