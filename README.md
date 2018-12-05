# git-speedup
Speed up git bash on windows, also works on Mac machines. Including improvement of speed of executing git status, git add, git pull, git push, ls, cd etc.
## Losing track of files
```
git branch --set-upstream YOUR_BRANCH_NAME upstream/YOUR_BRANCH_NAME
```
# Three Vesions 
### gitspeedup.sh ---- Small projects
Which disEnable the ignoreStat, can use the ```git add ``` when we have some changed
### gitspeedupL.sh ---- Large projects
Which enable the ignoreStat, please use the 
```git update-index --really-refresh
``` when we have changed files
### OptimazingSSH.sh ---- Any Projects which are using SSH 
Which optimazing the SSH setting

# How it works

#### 1. Repack the obects, the default was not to change the window or depth at all. As suggested by Jon Smirl, Linus Torvalds and others
```bash
git repack -a -d --depth=250 --window=250 
```
For more detail, please visit link:
http://git.661346.n2.nabble.com/git-repack-vs-git-gc-aggressive-td7564559.html

#### 2. Enable git preload file index
```bash
git config --global core.preloadindex true
```

#### 3. Avoid inspecting large working trees' modification times
```bash
git config core.ignoreStat true
```
When working with **large working** trees, Git's (frequent) checking whether files were modified since Git's internal index was last updated can lead to substantial lags. In such a case, it can make sense to switch off this check, but it comes at a price: it requires discipline on the developer's side to keep track which files were changed and git add them explicitly for the next commit (the output of git status will no longer identify modified files). You can disable the check per-repository thusly

When ```bash git mv ```and ```bash git rm``` are used, those changes do get noticed, even on assume-unchanged files. When new files are added, eg by git annex add, they are also noticed, then, 
please use following code instead ```bash git add ``` 
```bash 
git update-index --really-refresh
```
For more detail, please visit link: https://git-annex.branchable.com/tips/assume-unstaged/

#### 4. Enble file system cache for local
```bash
git config core.fscache true
```

#### 5. Enble file system cache for global
```bash
git config --global core.fscache true
```

#### 6. Cleanup unnecessary files and optimize the local repository
```bash
git config --global gc.auto 256
```

#### 7. Enables the sharing of multiple SSH sessions over a single network connection, and auto-creating a master connection if it does not already exist
```bash
ControlMaster auto
```
#### 8. Specifies the path to the control socket used for connection sharing. %r will be substituted by the remote login username, %h by the target host name and %p by the port.
```bash
ControlPath /tmp/%r@%h:%p
```
#### 9. Keeps the master connection open in the background indefinitely
```bash
ControlPersist yes
```
# How to use this

## Just run this sh file by double click

## Using code to run this in git
```bash
sh Script.sh
```
### Plan A
```bash
# Set the execution for all the users for the given script
chmod +x Script.sh

# Execute the script
./Script.sh
```

### Plan B
```bash
# Set the execution for all the users for the given script
chmod 777 /PATHTOFILE/Script.sh

# Execute the script
/PATHTOFILE/Script.sh
```

## Another way to make git faster (From online)
I found the following change on a StackOverflow post that seems to work way faster:

Go to your git install directory (Mine is c:\Program Files (x86)\Git
In the etc folder, open the file 'profile' in a text editor
This file is executed when the bash shell is initialized, you can do a lot of customization in here
Near the bottom, you will find some commands related to setting a variable called PS1 (PS1 = ...)
Either replace it or just append the following lines after it

```bash
fast_git_ps1 ()                                                                              
{                                                                                            
    printf -- "$(git branch 2>/dev/null | grep -e '\* ' | sed 's/^..\(.*\)/ {\1} /')"    
}                                                                                            

PS1='\[\033]0;$MSYSTEM:\w\007                                                                
\033[32m\]\u@\h \[\033[33m\w$(fast_git_ps1)\033[0m\]                                         
$
```

# Deleted Code

```bash
GIT_TRACE=1 git stash, this code was removed.
```
##### Activate Git's own tracing, because the Git bash executes subcommands, this will be executes internally
