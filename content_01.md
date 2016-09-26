
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
