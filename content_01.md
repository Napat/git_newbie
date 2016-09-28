
--------------------------------------------------------------
# CHAPTER01: Version control & git setup & status of staging 
Old story
- No time capsule (Time log and history tracking)
- Collaboration issues(i.e. Editing same source code, multi-tasking)

Version control systems
- Central Repository
 + Online(remote) repo only (i.e. subversion)
- Distributed Repository[Distributed version controlsystem: DVCS]
 + Support both Online(remote) and Offline(local) repository committing 

First, setup configuration for global/local repositories
```
# To set global config
git config --global username "Bumble Bee"
git config --global user.email chev@camaro.com
git config --global color.ui <auto/true>    # I prefer to use true
git help

# More tools
git config --global core.editor emacs
git config --global merge.tool opendiff

# To set local config(Local config will load after and override the global config)
git config user.email "Napat Rungruangbangchan"

# List config (Same key can set twice, the second will override the previous listing)
git config --list

# To verify running config of any repository
git config user.email  

```

Create new git repository
```
mkdir trygit
cd trygit

# Creates a new local repository
git init
```

## Git staging status
`git status` To see any status
- Untracked 
- Tracked
 + Unstaging area (changed/modified)
 + Staging area (ready to take snapshot)

- `git add <file01> <file01> ...` Move anything from any stage to *Staging area*
 + Add the list of files: `git add file1 file2 ...`
 + Add all file `git add --all`
 + Add all file in directory `git add source/`
 + Add all txt files in current directory `git add *.txt`
 + Add all txt files in any directory `git add docs/*.txt`
 + Use *Double quotes* to add all txt files in the whole project `git add "*.txt"`

## Commit
`git commit` Take snapshot of those staging
 + `-a` Automatic add changes from all tracked files
 + `-m` to comment, **Use present tense comment**

## Basic Log
```
git log
git log --oneline --graph
```

-----------------------------------------------------
# CHAPTER02: Reset, Diff, Unindex, Update-index and basic working with remote(1)

## Reset
- Unstage change using reset
 + `git reset <commit>` Undoes all commits after [commit] preserving changes locally
 + `git reset HEAD` Move all staged back to unstage area 
 + `git reset <file>` Unstages the file but preserve its contents
 
- Undoing a commmit using reset to the snapshot **Note: Don't do these after you push**
 + `git reset --hard HEAD^` Undo last commit and all changes
 + `git reset --soft HEAD^` Reset changes into staging
 
- Edit last commit(new commit message or add something) **Note: Whatever has been staged is added to last commit**
 ```
 git add somethingToLastCommit.txt
 git commit --amend -m "The new commit message"
 ```

- Discard changes in working(unstage)
 + `git checkout -- <file>` 

## Diff
- Check differences(`git diff`)
 + `git diff` , `git diff HEAD` Shows file differences not yet staged(unstage area)
 + `git diff --staged` Shows file differences between staging and the last commit

## Rm
- Remove/Unindex(`git rm ...`)
    + `git rm xyz.c`
    + `git rm --cached abc.log` If already tracked files and don't want to delete from local file system, only remove from git repository

## Update-index
- Update-index (**Settings only apply to local repository, other devs need to independent set Update-index configs**)
 + `git update-index --assume-unchanged pathToFile` Start ignoring the changes to the file  
 + `git update-index --no-assume-unchanged pathToFile` Keeping track again
 + `git ls-files -v|grep '^h'` Get a list of dir's/files that are assume-unchanged
 
# Working with REMOTE
 - `origin`: default first remote name

## Add remote(like bookmark in browser)
 - `git remote add origin <remote addr>`

## Show remote
 - `git remote -v`

## Push local to remote
 - `git push -u <remote name> <local branch name to push>`

Note `-u` to remember remote name so next time can type only `git push`

# Pull (fetch and merge to current branch)
 - `git pull`
 - `git pull <remote name>`
 
 i.e. 
 Merge the remote branch next into the current branch 
 - `$ git pull origin next`

 is the same as 
```
$ git fetch origin
$ git merge origin/next
```

-------------------------------------
# CHAPTER03: basic working with remote(2), Branching and merge

## Cloning
 1. Download to local dir
 2. Add `origin` remote pointing to clone URL
 3. Initial branch master and HEAD

 - `git clone <https://remoteUrl/repo-name.git>` Clones a project to local directory using repository name(default)

 - `git clone <https://remoteUrl/repo-name.git> localnamedir` Clones a project to specific local directory

## Branching

 - Create branch and checkout
```
git branch feature1
git checkout feature1
```

is the same as `git checkout -b feature1`

 - Merge branch 
```
git checkout master
git merge feature1
```

    + Fast-forward merge
        ++ By default when no change in current branch, git will merge by fast-forward method.
    + Non-fast-forward merge
        ++ `Recursive stratrgy`: If changes were made in both branches
        ++ Config force non-fast-forward merge<Optional>

## vi commands
```
`j` Down     `k`up    `ESC` leave mode  `:wq` save & quit
`h` left     `l`right `i` insert mode   `:q!` cancle & quit
```

 - Delete branch
    + `git branch -d <branch name>` delete only when already merging  
    + `git branch -D <branch name>` always delete the branch

--------------------------------------------------------------
