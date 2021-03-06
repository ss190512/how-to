## Create a new project(my_project). Add a new file into it and commit.
```
git init my_project
cd my_project
git status
vi new_file.txt
git status
git add new_file.txt
git status
git commit
git status
```
## Adding git to an existing project.
```
$ mkdir my_project
$ cd my_project/
$ touch {one,two,three,four,fife,six,seven}.txt
$ ls
fife.txt  four.txt  one.txt  seven.txt  six.txt  three.txt  two.txt
$ for ii in *.txt; do echo $ii; echo "First string."> $ii; done
fife.txt
four.txt
one.txt
seven.txt
six.txt
three.txt
two.txt
$ git init
Initialized empty Git repository in .../my_project/.git/
$ git status
On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        fife.txt
        four.txt
        one.txt
        seven.txt
        six.txt
        three.txt
        two.txt
nothing added to commit but untracked files present (use "git add" to track)
$ git add .   <-Add all files recursivly.
$ git status
On branch master
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   fife.txt
        new file:   four.txt
        new file:   one.txt
        new file:   seven.txt
        new file:   six.txt
        new file:   three.txt
        new file:   two.txt
$ git commit -m "My first commit"
[master (root-commit) e53fc4a] My first commit
 7 files changed, 7 insertions(+)
 create mode 100644 fife.txt
 create mode 100644 four.txt
 create mode 100644 one.txt
 create mode 100644 seven.txt
 create mode 100644 six.txt
 create mode 100644 three.txt
 create mode 100644 two.txt
$ ls -a
./  ../  .git/  fife.txt  four.txt  one.txt  seven.txt  six.txt  three.txt  two.txt
$ git status
On branch master
nothing to commit, working tree clean
$ rm -rf .git/
$ git status
fatal: not a git repository (or any of the parent directories): .git
```
## Clone git repository.
```
git clone <URL of repository(my_repository for example)>
cd my_repository
git status
```
## Push, pull commands.
```
git pull origin master
git pull 
git push origin master
git push 

alias gp='git pull && git push' 
```
## Add and commit in one command(can be done only with tracked files).
```
$ git commit -am "My first commit"

alias gac='git commit -am'
```
## List of tracked files.
```
$ git ls-files

alias gl='git ls-files'
```
## Unstage file.
```
git reset HEAD <file name>
```
## Discard changes in working directory.
```
git checkout -- <file name>
```
## Renaming by git.
```
git mv <old file> <new file>
git commit 
```
### Renaming by OS commands.
```
mv <old file> <new file>
git add -A 
git commit 
```
### Another way of renaming by OS commands.
```
mv <old file> <new file>
git add <new file>
git -u
git commit 
```
## Delete tracked files.
```
git rm <file name>
git commit
```
## Delete tracked file and return it back to the working directory.
```
git rm <file name>
git reset HEAD <file name>
git checkout -- <file name>
```
## Remove by OS command.
```
rm <file name>.txt
git add -A
git commit
```
