## Ctrl commands.
```
Ctrl+a or Home: Go to the beginning of the line.

Ctrl+b: Move backward one character.

Ctrl+d: Close the terminal. 

Ctrl+d or Delete: Delete the character under the cursor.

Ctrl+e or End: Go to the end of the line.

Ctrl+f: Move forward one character.

Ctrl+g: Leave history searching mode without running a command.

Ctrl+h or Backspace: Delete the character before the cursor.

Ctrl+j: Same as ENTER/RETURN key(Ctrl+M).

Ctrl+k: Cut all characters after the cursor and adding them to the clipboard.

Ctrl+l: Clear the screen. 

Ctrl+n or Down Arrow: Next command in history. 

Ctrl+o: Run a command you found with Ctrl+R.

Ctrl+p or Up Arrow: Previous command in history. 

Ctrl+q: Resume output to the screen after stopping it with Ctrl+S.

Ctrl+r: Searches the history backward.

Ctrl+s: Stop all output to the screen. 

Ctrl+t: Swaps the last two characters.

Ctrl+u: Cut the part of the line before the cursor, adding it to the clipboard.

Ctrl+v: Makes the next character typed verbatim.

Ctrl+w: Cut the word before the cursor, adding it to the clipboard.

Ctrl+xx: Move between the beginning of the line and the current position of the cursor. 

Ctrl+y: Paste the last deleted item(yank).

Ctrl+_: Undo your last key press. Can be repeated multiple times.

Ctrl+[: ESC key.
```
## Alt commands.
```
Alt+a: Go to the beginning of a line.

Alt+b: Go left one word.

Alt+c: Capitalize the character under the cursor. The cursor moves to the end of the current word.

Alt+d: Delete the word after the cursor.

Alt+f: Go right one word.

Alt+l: Uncapitalize every character from the cursor to the end of the current word.

Alt+r: Revert any changes to a command pulled from your history.

Alt+t or esc+t: Swap the current word with the previous word.

Alt+u: Capitalize every character from the cursor to the end of the current word.

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
