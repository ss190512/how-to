## Get the directory name from the full file path.
```
$ ls /c/ss/how-to/bash/bash_commands.md
/c/ss/how-to/bash/bash_commands.md
$ cd !$:h
cd /c/ss/how-to/bash
$ ls bash_commands.md
bash_commands.md
```
## Take arguments from the previous command.
```
$ grep something file1
something
$ egrep !:1-$
egrep something file1
something
$ ls !:2-$
ls file1
file1
$ ls !$
ls file1
file1

mkdir qqq
cd !$

$ touch {one,two,three,four}.txt five.txt six.txt
$ chmod 777 !*
chmod 777 {one,two,three,four}.txt five.txt six.txt
```
## !beginning_of_the_command - Run the last command stared with beginning_of_the_command.
## Making a backup or rename the file.
```
cp text.txt{,_bkp}
mv qqq{,.bkp}
```
### Make a backup and compare it with the original file.
```
cp biggest_segments.sql{,.bkp}
diff biggest_segments.sql{.bkp,}
2d1
< -- Some comments
```
### Create multiple files and change extension of one.txt.
```
touch {one,two,three,four}.txt
mv one.{txt,bkp}
```
### Other examples with {}.
```
echo {one,two,three,red}" something,"
one something, two something, three something, red something,

echo {one,two,three,red}" green"
one green two green three green red green

echo {{1,2,3,4,5},1,2,3,4,5}
1 2 3 4 5 1 2 3 4 5
```
## Take the last command, replace the string, and execute it.
```
$ lm text.txt
-bash: lm: command not found
$ ^lm^ls
ls text.txt
text.txt
```
## List of sub-directories.
```
ls -d */
ls -d1 */
ll -d */
lr -d */
Where: alias lr='ls -ltr'
```
## pushd and popd commands.
```
pushd
pushd .
alias pd="pushd "
popd
pushd || popd
alias pp="pushd || popd"
dirs
dirs -v
alias dirs="dirs -v"
cp text.txt ~#
ls ~#
cd ~#
```
## Tar and zip bash function.
```
tz () {
    if [ -z "$1" ]; then
          echo "No input parameter. Exit."
    else
        echo "Compressing $1"
        tar cf - ./$1 | gzip -9 > $1.tar.gz
        ls -la ./$1.tar.gz
    fi
}
```
## Command substitution.
```
rpm -ql $(rpm -qa | grep color)
/usr/share/doc/hicolor-icon-theme-0.11
...
```
## Ignore errors.
```
find / -name something 2>/dev/null
```
## Equivalent commands.
```
find / -name something > output.txt
find / -name something 1> output.txt
```
## Redirect both output and errors into the same file.
```
find / -name something > output.txt 2> output.txt
find / -name something > output.txt 2>&1
find -name test.txt 2>&1 | tee /tmp/output1.txt
```
## Find grep combination. Find import in all pythin files. 
```
find . -name "*.py" -exec grep -Hi import '{}' ';'
```
## For loop.
```
for f in * ; do echo $f; done
```
## Commands that start with space do not go in the history.
```
export HISTIGNORE="[ \t]*"
$ ls
$history
```
## fc - open the last command in the editor and run it after editing. 
## Type, file, and command commands.
```
$ type lr
lr is aliased to `ls -lrt'
$ type ping
ping is /c/windows/system32/ping
$ type time
time is a shell keyword
$ type pushd
pushd is a shell builtin

$ file $(which ls)
/usr/bin/ls: PE32+ executable (console) x86-64 (stripped to external PDB), for MS Windows
$ file bash_commands.md
bash_commands.md: ASCII text, with CRLF line terminators

$ command -V ls
ls is aliased to `ls -F --color=auto --show-control-chars'
$ command -V ll
ll is aliased to `ls -l'
$ command -V cd
cd is a shell builtin
$ command -V find
find is hashed (/usr/bin/find)
```
## Grep examples.
### grep -iRl text * - Run grep recursively, ignoring uppercase, and showing only file names.
### Count the occurrence of a word.
```
$ grep -c import ser_lib.py
31
``` 
## Useful aliases.
```
alias g='git'
alias ga='git add'
alias gc='git commit -m'
alias gac='git commit -am'
alias gp='git pull && git push'
alias gs='git status'
```
## Put regexp in the alias.
```
alias my_reg='echo [0-9a-f]...
...| grep -E ${my_reg}
```
## && works the same as ";" but it runs the next one only if the previous one was successful.
```
sudo apt update && sudo apt upgrade
```
## String manipulations.
```
$ var="Beginninggddd  of the string."
$ new_var="${var#Beginning}"
$ echo $new_var
of the string.

$ var="Beginning of the string. End its end."
$ new_var="${var% End its end.}"
$ echo $new_var
Beginning of the string.
```
## Default values.
```
$ first_arg="${1:-no_first_arg}"
$ echo ${first_arg}
no_first_arg
```
## Traps.
```
function cleanup() {
    echo "Working on cleanup now."
} 
trap cleanup TERM INT QUIT
```
## Random variable.
```
$ echo ${RANDOM}
4410

$ echo ${RANDOM}${RANDOM}${RANDOM}
104652305316628

$ my_temp_file=./temp_file_${RANDOM}
$ touch ${my_temp_file}
$ ls ${my_temp_file}
./temp_file_623
```
## REPLY variable.
```
$ read
Hello!
$ echo ${REPLY}
Hello!
```
## More embedded variables.
```
$ echo $LINENO; echo ${SECONDS}; sleep 2; echo ${SECONDS}
67
23276
23278
```
## Exit in 10 secs.
```
TMOUT=10
```
## Associative arrays(dictionaries).
```
$ declare -A my_dic=([one]=1 [two]=2)
$ echo ${my_dic[one]}
1
$ my_dic[one]="1111111111111111"
$ echo ${my_dic[one]}
1111111111111111
$ my_key=two
$ echo ${my_dic[$my_key]}
2
```
## "<()" command.
```
$ grep "my string" file1 > f1
$ grep "my string" file2 > f2
$ diff f1 f2
0a1
> my string

$ diff <(grep "my string" file1) <(grep "my string" file2)
0a1
> my string
```
## z commands(zless, zcat, zgrep etc.) for working with compressed files. 
## Enable grep colors.
```
export GREP_OPTIONS='--color=auto'
```
## Add colors to Less/Man pages
```
export LESS_TERMCAP_mb=$'\e[01;31m'       # blinking
export LESS_TERMCAP_md=$'\e[01;38;5;74m'  # bold
export LESS_TERMCAP_me=$'\e[0m'           # end mode
export LESS_TERMCAP_se=$'\e[0m'           # end standout-mode
export LESS_TERMCAP_so=$'\e[38;5;246m'    # standout-mode - info box
export LESS_TERMCAP_ue=$'\e[0m'           # end underline
export LESS_TERMCAP_us=$'\e[04;38;5;146m' # begin underline
```
## Check port by echo.
```
-bash-4.4$ echo >/dev/tcp/server_name/22

-bash-4.4$ echo >/dev/tcp/server_name/23
-bash: connect: Connection refused
-bash: /dev/tcp/vs-iapp752/23: Connection refused
```
## Kill with grep. For example kill all oracle processes.
```
kill -9 $(ps -awxu | grep -i oracle | grep -v grep | cut -c 9-14)
```
