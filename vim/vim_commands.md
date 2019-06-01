## Run vim without sourcing vimrc and with 'nocompatible' option.
```
$ vim -u NONE -N
```
## Minimum VIM configuration required to activate built-in plugins.
```
set nocompatible
filetype plugin on
```
## Run custom profile instead of vimrc.
```
vim -u my_profile.vim
```
## Check path of vimrc. 
```
:echo $MYVIMRC
```
## Check version of VIM which shows the configuration of the build - small, normal, big, huge.
```
:version
...
Huge version without GUI. Features included (+) or not (-):
...

:h +feature-list
```
## Help of GUI vim.
```
:h gui
```
## Dot command(micro macro).
```
:h .
```
## Increases the indentation from the current line until the end of the file: >G
## Append a semicolon at the end of each line multiple times.
```
A;<Esc>
j.
j.
```
## Condensed versions of some commands.
```
c$ C
cl s
^C S
^i I
$a A
A<CR> o
ko O
```
## Replace "+" to " + " multiple times.
```
f+
s + <Esc>
;.
;.
```
## Act, Repeat, Reverse.
## Repeat/Reverse commands.
```
. u Make a change and reverse.
; , f{char} / t{char} Scan line for next character
; , F{char} / T{char} Scan line for previous character
n N /pattern<CR> Scan document for next match
n N ?pattern<CR> Scan document for previous match
& u :s/target/replacement or :substitute/target/replacement Perform substitution
@x u qx{changes}q Execute a sequence of changes
```
## Search and replace with * command.
```
:set hls
*
cw<new word><Esc>
n
.
```
### Alternative way to do it.
```
:%s/<old_word>/<new word>/g 
```
## Delete word backward/forward/in one command(delete a word mnemonic).
```
db
x

b
dw

daw
```
## Count numbers(# is a number below).
```
<C-a>
<C-x>
#<C-x>
#<C-x>
```
