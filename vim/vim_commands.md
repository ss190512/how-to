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
