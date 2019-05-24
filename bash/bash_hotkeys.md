## Ctrl commands.
```
Ctr+a or Home: Go to the beginning of the line.
Ctr+b: Move backward one character.
Ctr+d: Close the terminal. 
Ctr+d or Delete: Delete the character under the cursor.
Ctr+e or End: Go to the end of the line.
Ctr+f: Move forward one character.
Ctr+g: Leave history searching mode without running a command.
Ctr+h or Backspace: Delete the character before the cursor.
Ctr+j: Same as ENTER/RETURN key(Ctrl+M).
Ctr+k: Cut all characters after the cursor and adding them to the clipboard.
Ctr+l: Clear the screen. 
Ctr+n or Down Arrow: Next command in history. 
Ctr+o: Run a command you found with Ctrl+R.
Ctr+p or Up Arrow: Previous command in history. 
Ctr+q: Resume output to the screen after stopping it with Ctrl+S.
Ctr+r: Searches the history backward.
Ctr+s: Stop all output to the screen. 
Ctr+t: Swaps the last two characters.
Ctr+u: Cut the part of the line before the cursor, adding it to the clipboard.
Ctr+v: Makes the next character typed verbatim.
Ctr+w: Cut the word before the cursor, adding it to the clipboard.
Ctr+xx: Move between the beginning of the line and the current position of the cursor. 
Ctr+y: Paste the last deleted item(yank).
Ctr+_: Undo your last key press. Can be repeated multiple times.
Ctr+[: ESC key.
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
### Vi mode:
```
set -o vi 
```
### Emacs mode:
```
set -o emacs 
```
