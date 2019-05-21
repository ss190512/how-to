## Repeating the last argument in scripts.
```
mkdir qqq
cd !$
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
## Show the type of program.
```
$ type lr
lr is aliased to `ls -lrt'
$ type ping
ping is /c/windows/system32/ping
$ type time
time is a shell keyword
$ type pushd
pushd is a shell builtin
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
alias gp='git push'
alias gs='git status'
```
## Put regexp in the alias.
```
alias my_reg='echo [0-9a-f]...
...| grep -E ${my_reg}
```
## && works the same as ;
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

