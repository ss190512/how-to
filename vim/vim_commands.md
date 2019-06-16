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
## Operator Commands.
```
Trigger Effect
--------------------
c  Change 
d  Delete 
y  Yank into register 
g~ Swap case 
gu Make lowercase 
gU Make uppercase. gUgU or gUU - uppercase the line.
>  Shift right
<  Shift left
=  Autoindent
!  Filter {motion} lines through an external program 
```
## Treat numbers as decimal.
```
set nrformats=
```
## Intend/auto-intend the whole file.
```
gg>G
gg<G
gg=G
```
## Insert mode and Vim’s command line short keys.
```
<C-h>: Backspace.
<C-w>: Del back one word.
<C-u>: Del back to the beginning of line.
```
## Switch short keys.
```
<C-[>: Switch to normal mode.
<C-o>: Switch to insert normal mode.
```
## Scroll the screen in the middle in the insert mode.
```
<C-o>zz
```
## Insert from the default buffer in the insert mode.
```
<C-r>0
<C-r><C-p>0
```
## Expression register.
```
<C-r>=
```
## Numeric code for a character.
```
ga
```
## Entering characters from the insert mode.
```
<C-v>{123}: By decimal code.
<C-v>u{123}: By hexadecimal code.
<C-v>{nondigit}: Nondigit literally.
<C-k>{char1}{char2}: Represented by {char1}{char2} digraph(:h digraph-table). 
```
## Replace/virtual replace, single character/single character virtual replace modes.
```
R/gR
r/gr
```
