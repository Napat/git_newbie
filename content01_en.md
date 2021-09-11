# git newbie tutorial

## Sublime packages

- [GitGutter](https://github.com/jisaacks/GitGutter)
  
## PowerShell

- [posh-git: Git-Autocomplete for PowerShell](https://github.com/dahlbyk/posh-git)  

## Graphical git client

- [GitKraken](https://www.gitkraken.com/) Windows/Linux/OSX. Free for personal use and lightweight.
- [SmartGit](http://www.syntevo.com/smartgit/) Windows/Linux/OSX. Free if being used for private projects not meant to be profitable and not the opensource license.
- [SourceTree](https://www.sourcetreeapp.com/) Windows/OSX. Free Git client.  
- [TortoiseGit](https://tortoisegit.org/) Windows. Free Git client familiar with TortoiseSVN.
- [GithubDesktop](https://desktop.github.com/) Windows/OSX. Free and easy to use.

## Browser Extensions

- [Octotree](https://github.com/buunguyen/octotree) Browser extension (Chrome, Firefox, Opera and Safari) to show a code tree on GitHub.  
- [GitHub1s](https://chrome.google.com/webstore/detail/github1s/lodjfmkfbfkpdhnhkcdcoonghhghbkhe/)
  
## Useful tools

- [GitIgnoreIO](https://www.gitignore.io/) To create .gitignore files for your project.  
  
## Github hotkey

- `t`: push `t` when open target repository website then ypu can type any text to search for any file on that repository.  
  
--------------------------------------------------------------

## CHAPTER01: Version control & git setup & git config & status of staging 

Old story

- No time capsule snapshot(Time log and history tracking)
- Collaboration issues(i.e. Editing same source code, multi-tasking)

![alt tag](img/old_story.jpg)

### Version control systems

Central Repository

- Repository what?
- Online(remote) repo only, i.e. subversion

Distributed Repository[Distributed version control system: DVCS]

- Support both Online(remote) and Offline(local) repository committing

![alt tag](img/what_version_control_graph_looklike.jpg)

### First to know, Git config

Git configuration use to keep many config like developer's name, email, status coloring, diff, tools, alias commands and more.
When executing, git then look for config at 3 levels location.
There are 4 options to select config write location,
`--system`, `--global`, `--local` and `--file <filename>`.

`git config --list --show-origin`: To see shich config is set where

`git config --system`: Write config for entire system(all users/projects).

- Config stored in `/etc/gitconfig`

`git config --global`: Write config for all projects for current user.

- Config stored in `~/.gitconfig`(or `~/.config/git/config`) file

`git config` or `git config --local`: By default, git config values are writed to current project config file.

- Config stored in `<project dir>/.git/config`.

`git config --file <filename>`: There is optional write config to any specific path, maybe to use when replace export linux's env to non-standard values. See Somehack belows.

The first place Git looks for the values is `system` level, the next is `global` level.
Finally, Git looks for configuration values in `local`(project) level.

*Project overrides Global and Global overrides System.*

Shortcut to edit config

``` bash
git config --global -e    # edit global config
git config --local -e     # edit local config
```

We can also point the environment variable `GIT_CONFIG` to a file that git config should use. With `GIT_CONFIG=~/.gitconfig-A git config key value` the specified file gets manipulated.

For example to setup configuration for global/local repositories

``` bash
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

Useful config to set color of git status

``` bash
[color "status"]  
  added = green bold  
  changed = yellow bold  
  untracked = red bold
```

Create new repository

``` bash
mkdir trygit
cd trygit

# Creates a new local repository
git init

# Folder .git is created after git init command
ls -a | grep .git
drwxrwxrwx 2 root root     0 Jun 30 18:09 .git
```

### Git staging status

`git status` To see any status

1. Untracked

2. Tracked

- Unstaging area (changed/modified)
- Staging area (ready to take the snapshot)

To move anything from any stage to *Staging area*  
`git add <file01> <file01> ...`

- Add the list of files: `git add file1 file2 ...`
- Add all file `git add --all`
- Add all file in directory `git add source/`
- Add all text files in current directory `git add *.txt`
- Add all txt files in any directory `git add docs/*.txt`
- Use *Double quotes* to add all text files in the whole project `git add "*.txt"`

### Commit

`git commit` Take snapshot of those staging

- `-a` Automatic add changes from all tracked files
- `-m` to comment, **Use present tense comment**  

#### Basic commit

``` bash
git commit -a -m "your messages"
```

#### Using commit template: communication/collabolation/documentation

1. Create `commit_msg_template.txt` template  

``` bash
$ cat commit_msg_template.txt
# Some awesome title (concise, ideally in 50 characters or fewer)

# Body (bug/issue tracking no/feature description)

# Documentation/ Notes
```

2. Set project template  

``` bash
$ cd <project_dir>
$ cp </path/of/commit_msg_template.txt> commit_msg_template.txt
$ git add commit_msg_template.txt
$ git config commit.template commit_msg_template.txt
$ git commit
... 
```

3. Check setting

``` bash
$ cat .gitconfig | grep template 
template = commit_msg_template.txt
```

### Basic Log

``` bash
$ git log
$ git log --oneline --graph
...
```

-----------------------------------------------------

## CHAPTER02: head, HEAD, Reset, Diff, Unstage,Unindex, Update-index and basic working with remote(1)

### head

A head is simply a reference to a commit object.
Each head has a name (branch name or tag name, etc).
By default, there is a head in every repository called master.
A repository can contain any number of heads.

### HEAD

HEAD(all capitals head) is the exclusive head to reference to the current position. 
To see what HEAD points to by doing

``` bash
$ cat .git/HEAD
ref: ref/heads/master
```

#### Detached HEAD

It is possible for HEAD to refer to a specific revision that is not associated with a branch name. 
This situation is called a [detached HEAD](https://git-scm.com/docs/git-checkout#_detached_head).

### Reset

Unstage change using reset

- `git reset <commit>` Undoes all commits after [commit] preserving changes locally
- `git reset HEAD` Move all staged back to unstage area 
- `git reset <file>` Unstages the file but preserve its contents

Undoing a commmit using reset to the snapshot **Note: Don't do these after you push**

- `git reset --hard HEAD^` Undo last commit and all changes
- `git reset --soft HEAD^` Reset changes into staging
- `git reset HEAD <file>...` To unstage file

Edit last commit(new commit message or add something) **Note: Whatever that has been staged is added to last commit**

``` bash
git add somethingToLastCommit.txt
git commit --amend -m "The new commit message"
```

Change author and Email of last commit

``` bash
git commit --amend --author="NewFirstname NewLastname <NewEmail@domain>"
```

Discard changes in working(unstage)  
`git checkout -- <file>`

### Diff

Check differences(`git diff`)

- `git diff` , `git diff HEAD` Shows file differences not yet staged(unstage area)
- `git diff --staged` Shows file differences between staging and the last commit

### Rm

Remove/Unindex(`git rm ...`)

- `git rm xyz.c`
- `git rm --cached abc.log` If file is already tracked and you don't want to delete the changed from local file system. Only unindex the file from git repository.

### Update-index

Update-index (**Settings only apply to local repository, other devs need to independent set Update-index configs**)

- `git update-index --assume-unchanged pathToFile` Start ignoring the changes to the file  
- `git update-index --no-assume-unchanged pathToFile` Keeping track again
- `git ls-files -v|grep '^h'` Get a list of dir's/files that are assume-unchanged

## Working with REMOTE

- `origin`: default first remote-name

### Add remote(like bookmark in browser)

- `git remote add origin <remote addr>`

### Show remote

- `git remote -v`  
- `git remote show`  
- `git remote show origin`  

### Push local to remote

- `git push -u <remote name> <local branch name to push>`

Note `-u` to remember remote name so next time can just type `git push`

## Pull (fetch and merge to current branch)

- `git pull`
- `git pull <remote name>`

i.e. Merge the remote branch `next` into the current branch

- `$ git pull origin next`

It is the same as,

``` bash
$ git fetch origin
$ git merge origin/next
```

--------------------------------------------------------------

## CHAPTER03: basic working with remote(2), Branching and merge

### Cloning

1. Download to local dir
2. Add `origin` remote pointing to clone URL
3. Initial branch master and HEAD

- `git clone <https://remoteUrl/repo-name.git>` Clones a project to your local filesystem using repository name(default)

- `git clone <https://remoteUrl/repo-name.git> localnamedir` Clones a project to the specific local directory

### Branching

- Create branch and checkout

``` bash
git branch feature1
git checkout feature1
```

is the same as `git checkout -b feature1`

[Git 2.23](https://github.com/git/git/blob/master/Documentation/RelNotes/2.23.0.txt) came up with the new [git switch](https://git-scm.com/docs/git-switch) command, 

#### TL;DR: git checkout vs git switch

To switch from one branch to another

`git checkout <branchname>` or `git switch <branchname>`

To create a new branch and also switches to it

`git checkout -b <new_branchname>` or `git witch -c <new_branchname>`

- Merge branch

``` bash
git checkout master
git merge feature1
```

**Fast-forward merge**  
By default when no change in current branch, git will merge by fast-forward method.

**Non-fast-forward merge**  
`Recursive stratrgy`: If changes were made in both branches.  
Config force non-fast-forward merge. [Optional]  

### vi commands

``` bash
`j` Down     `k`up    `ESC` leave mode  `:wq` save & quit
`h` left     `l`right `i` insert mode   `:q!` cancle & quit
```

Delete branch

- `git branch -d <branch name>` delete only when already merging  
- `git branch -D <branch name>` always delete the branch

--------------------------------------------------------------

## CHAPTER04: Basic Collaboration

git work flow: Dev work only on master branch

``` text
*Senario1*
A: push
B: clone
A&B: edit (different code segments)
A:push
B:push ----> !! error, rejected
B: pull (using: fetch & merge--> Git work flow: merge commit strategy)
B: push
```

``` text
*Senario2*
A: push
B: clone
A&B: edit (same code segments)
A:push
B:push ----> !! error, rejected
B: pull (using: fetch & merge) ----> !! content Conflict 
B: git status
    & see conflict files 
    & talk to A 
    & manual fixed conflict
B: git commit -a -m "Fixed conflict bla bla..."
B: push
```

- Merge commit strategy
Log graph then looks something like
![alt tag](img/gitflow01_onlyBranch.jpg)

### Avoiding Git Disasters

Merging may feels like tons of pain when there are tons of changes long time no sync.
![alt tag](img/merging_feels_like.gif)

--------------------------------------------------------------

## CHAPTER05: remote branch & tags

keyword: stale tracking branches issue

Push local branch to remote
`git push origin <feature01>`

To see all remote branches

``` bash
# Need to sync with remote
git pull
git branch -r
git checkout <feature01>
```

Or (prefered)

``` bash
# No need to sync with remote
git remote show origin
```

Remove a remote branch

- `git push origin :<branchName>`

or  

- `git push origin --delete <branchName>`

Note: The other developers that already sync the deleted branch will have **stale tracking branches** issue.
To clean local reference(origin) with deleted remote branches we can use *prune*, i.e., `git fetch -p`.

### remote prune

`git fetch -p` or `git remote prune origin`

Senario 5.1

``` text
A&B: git pull origin feature01
A: git push origin --delete feature01
B: new commit & git push ----!!! Everything up-to-date (stale tracking branches)
B: git remote show origin ----(see stale remote branches)
B: git remote prune origin
```

To track different local branch name to remote branch name
`git push origin <localBranch>:<remoteBranch>`

Ex: push staging branch to master

``` bash
git checkout staging
git push heroku-staging staging:master
```

Tagging (reference to a commit used mostly for release versioning)

List all tags
`git tag`

Checkout to any tag
`git checkout <tagName>`

To add a new tag
`git tag -a <tagName> -m "Comment about this tag"`

To push news tags
`git push --tags`

--------------------------------------------------------------

## CHAPTER06: rebase

Tag: Git work flow, Rebase strategy

![alt tag](img/gitflow02_withRebase.jpg)

Rebase do three things
    1. Move all current working branch changes(commits) to temporary area.
    2. Apply all base branch commits to working branch.
    3. Apply all commits from temporary area one at a time.

Rebase conflict commands 

After resolved the issue then continue with commands

``` bash
git status
git add <resolve files>
git rebase --continue
```

If you want to skip this patch(skip only this commit)

``` bash
git rebase --skip
```

To abort the rebase process

``` bash
git rebase --abort
```

--------------------------------------------------------------

## CHAPTER07: history, log, diff, blame, gitignore file, explicit repository excludes, Aliases

Info: `git help log`

### Git log

``` bash
git log
git log --graph
git log --pretty=oneline
git log --oneline
git log --graph --oneline
```

Log patch(diff) using `-p`

``` bash
git log --oneline -p
```

Log ranges By latest commits

``` bash
git log -3
```

Log ranges By time

``` bash
git log --until=1.minute.ago
git log --since=1.day.ago
git log --since=1.hour.ago
git log --since=1.month.ago --until=2.weeks.ago
git log --since=2010-01-01 --until=2011-12-31
```

Log format

``` bash
git log --pretty-format:"%h %ad- %s [%an]"
```

``` bash
Placeholder     Replace with
%ad             author date
%an             author name
%h              SHA hash
%s              subject
%d              ref names
```

Log stat(line change) using `--stat` 

``` bash
git log --oneline --stat
```

### Git diff

``` bash
git diff              # same as git diff HEAD
git diff HEAD^
git diff HEAD^^ HEAD^
git diff HEAD~2 HEAD~1
git diff HEAD~2 HEAD -- main.c  # show only diff of main.c file

git diff f5abca ed189a

git diff master feature01

git diff --since=1.week.ago --until=1.minute.ago
```

Note `--` use to tell git the following argument is the file name.

### Git blame

To see the commits that change any files

``` bash
git blame main.c --date short
```

## Git ignore

Excluding from all copies of cloning by add the rules to `.gitignore` file.
We need to add .gitignore file to the repository.

### Explicit repository excludes

It you don't want to create a `.gitignore` file to share with others,  
you can create rules that are not committed with the repository  
by write the exclude list into `.git/info/exclude`
No need to add .gitignore file to the repository.

### Aliases

``` bash
# git mylog
git config --global alias.mylog "log --pretty=format: '%h %s [%an]' --graph"

# git lol
git config --global alias.lol "log --graph --decorate --pretty=oneline -a"

# git st
git config --global alias.st status

# git co
git config --global alias.co checkout

# git br
git config --global alias.br branch

# git ci
git config --global alias.ci commit
```
  
Some alias commands are very hard by `git config` for example, alias of `git standup`, we could manual add alias to `.git/config` by adding:  

``` bash
[alias]
        standup = !"git log --reverse --branches --since='$(if [[ "Mon" == "$(date +%a)" ]]; then echo "last friday"; else echo "yesterday"; fi)' --author=$(git config --get user.email) --format=format:'%C(cyan) %ad %C(yellow)%h %Creset %s %Cgreen%d' --date=local"
```
  
``` bash
# git config
[alias]
search = log --no-merges -i -E --pretty='%h (%ad) - [%an] %s %d' --date=format:'%b %d %Y' --grep
vgrep = grep --extended-regexp --break --heading --line-number

# To use search
$ git search "pattern"
$ git grep "pattern"
```
  
--------------------------------------------------------------

## CHAPTER08: Create bare git repository
  
### Basic concept of repository

There are two types of git repository.

1. Repository for working (can changes source code)  
2. Repository of sharing (cannot change any source code): **usually on a remote server**  
  
### Repository for working

Typeiclly, when we use `git init` command at the top level. We can create working repository.
After that, we got two things at the top directory,

1. A `.git` subfolder with all the git related revision history of your repo  
2. A working tree(source code) or checked out copies of your project files.  
  
### Repository of sharing

Sometimes called, `bare repo` that the structured a bit differently from `woking repository`, `bare repos` store git revision history in the root folder instead of in a .git subfolder.  
  
A bare repository created with `git init --bare` is for sharing/collaborating/remoting with a team and developers to push their changes to `bare repo`.
Thus, Git is a distributed version control system, so **no one will directly edit files in the shared centralized(bare) repository**.
Developers can clone/push/pull the shared bare repo to make their changes available to other users.  
Note that bare repo contained no working copy of any source code files but store only resources to keep revision history.

``` text
[Local repository:for working] <---ssh/http/https/ftp/.../---> [Remote/Bare repository: for sharing] 
```

### To create bare repository  

``` bash
Ref 
- https://stackoverflow.com/questions/3242282/how-to-configure-an-existing-git-repo-to-be-shared-by-a-unix-group  
```
  
Server

- IP Address: 192.168.10.85  
- SSH: enable  
- Git workspace: /project/GitRepositories  
- Project name: example_project  
- My user: napat
- Linux group for user to access bare repo: git_group

``` bash
$ ssh://my_account@192.168.10.85
$ cd /project/GitRepositories
$ mkdir example_project.git
$ chown napat:git_group switch.git 
$ chown napat:git_group -R ./*
$
$ chgrp -R git_group example_project.git            # set the group
$ chmod -R g+rw example_project.git                 # allow the group to read/write
$ chmod g+s `find example_project.git -type d`      # new files get group id of directory
$
$ cd my_project.git
$ git init --bare --share 
$ 
```

--------------------------------------------------------------

## Dropbox bare repository

``` bash
Ref 
 - https://github.com/anishathalye/git-remote-dropbox
```

1. Open [link](https://www.dropbox.com/developers/apps), Create "Dropbox API app folder" and generate an "OAuth2 token"

2. Save "OAuth2 token" to `~/.config/git/git-remote-dropbox.json` with pattern,  

``` bash
{
    "default": "ReplaceThisStringToYourOAuth2Token"
}
```

3. Create project and init git repository used to push to dropbox  

``` bash
$ mkdir project01
$ cd project01
$ echo "Project for test" > README.md
$ git init
$ git add .
$ git commit -m "First commit"
...
```

4. Add remote url (`dropbox:///folderNameToUse`) and test push to dropbox 

``` bash
$ git remote add origin dropbox:///project01
$ git push origin master 
```

Now, folder "project01", bare repository, will be created in your dropbox app folder and ready to clone with 

More features

- [Multiple Accounts and token inline url](https://github.com/anishathalye/git-remote-dropbox#multiple-accounts)

--------------------------------------------------------------

## Collaborate strategy(Git workflow): Rebase and No-Fast-forward strategy

### Developers work only on master-branch(and no strategy)

This is not a practice, only for the beginner or lonely developer.

### Merge commit strategy

Still for not bad, messy log graph
![alt tag](img/gitflow01_onlyBranch.jpg)

### Rebase strategy

This strategy is good with a simple log graph
![alt tag](img/gitflow02_withRebase.jpg)

### No-Fast-forward strategy

- Easy to checkout history when buggy
![alt tag](img/gitflow03_noFastForward.jpg)

**Note** Dev still should smaller the task to be fast-feedback collaboration.
![alt tag](img/gitflow03_2_slowFeedbackProb.jpg)

### No-Fast-forward strategy setup

``` bash
# Config no fast-forward when merge the master branch
git config branch.master.mergeoptions  "--no-ff"
# or stop using no fast forward for all merging by  
git config --add merge.ff false

# Fixed git pull(fetch & merge) to be fetch & rebase 
git config pull.rebase true

# Short summary
cd <.git repository>
git config branch.master.mergeoptions "--no-ff"
git config pull.rebase true
```

### Things to do EVERY morning

``` bash
git checkout master
git pull origin master
git checkout <branch_dev>
git rebase master
```

### Before go home

- Please push any commits and resources that other developers may use for new release.

--------------------------------------------------------------

## Collaborate, command for reviwer to see diff between feature and mainline branch  

``` text
Ref: `https://stackoverflow.com/questions/20808892/git-diff-between-current-branch-and-master-but-not-including-unmerged-master-com`
```

When the reviewers have to check the change between feature and mainline.
They can use  "git diff" with hash of base commit. There are some easy way to fine that bash hash,
Let's say our feature branch name is `feature1` that branching from `master` branch. We can get hash of `branch point` by using erge-base,

``` bash
$ git merge-base feature1 master
fba7c14598a3e36f7a4ee0b956b68d21d64c195d
```

So, to get all change of `feature1` with `branch point` we can use,

``` bash
# To diff feature1 against basis
git diff `git merge-base feature1 master`..feature1

# or
git diff $(git merge-base feature1 master)..feature1

# or 
git diff $(git merge-base --fork-point master dev)..dev

# or if current branch is already on feature1
git diff `git merge-base feature1 master`
```

--------------------------------------------------------------

## Merging Two(or more) Git Repositories Into One Repository Without Losing File History

``` bash
# 1.Create a new empty repository New
mkdir newrepo
cd newrepo
git init   

# 2.Make a dummy commit because we need to have  an initial commmit before we do a merge.
touch dummy.txt
git add .
git commit -m "Init dummy commit"

# 3.Add a oldProjA remote and fetch the old repo to new branch
git remote add -f oldProjA <oldProjA repo URL>
git checkout oldProjA/master
git checkout -b editOldProjA

# 4.Move projectA's objects to new dir to prevent merge conflict with other project
mkdir <projADir>
git mv <all projA files and folder> <projADir>
git commit -m "Move projA to new dir"

# 5.merge projA to master branch using unrelated histories option
git checkout master
git merge --allow-unrelated-histories newb_proA

# Do the same approach(3-5) to other repository
#...
```

--------------------------------------------------------------

## Some hack: How to set .git directory to difference folder with source code

``` text
Ref: 
- http://stackoverflow.com/questions/17913550/git-moving-the-git-directory-to-another-drive-keep-the-source-code-where-it
```

Options  

1. Use `symlink`(linux) or `junction.exe`(windows)
2. Clone with option --separate-git-dir
`git clone --separate-git-dir=<path to directory for repo> <remote url> <path for working copy>`
3. Set up using `--git-dir=<path>` and `--work-tree=<path>`
4. Using same approach with submodules
    4.1 Move .git directory to where it need to be(ex: `~/googledrive/.git`).   
    4.2 Create .git file with value: `gitdir: ~/googledrive/.git`
    4.3 Define core.worktree to point at the working space

``` bash
# Option 4 example
cd ~/workspace/project_foo
mv .git ~/googledrive/.git
echo "gitdir: ~/googledrive/.git" > .git
git config core.worktree $PWD
```

*Note*  
Imagine when you move `.git` to the cloud storage directory to automatic backup files or some folders.
You may sync your repositories without setting up the remote server.  
By the way, this hack is just for fun and not recommended. xD

--------------------------------------------------------------

## Patching Linux Kernel source code

After create new branch(patch_branch) and commit source code with [Sign-off requirement](http://stackoverflow.com/questions/1962094/what-is-the-sign-off-feature-in-git-for)

### Create xxx.patch, similar to diff of master & patch_branch

`git format-patch master..patch_branch`

### To check xxx.patch using kernel script

`<linuxkernel>/scripts/checkpatch.pl xxx.patch`

### Get linux kernel patch maintainer email

`<linuxkernel>/scripts/get_maintainer.pl xxx.patch`

### Send email by git

`git send-email --to xxx@xxx.com *.patch`

--------------------------------------------------------------

## Get URL to direct access raw file on github.com and use it as CDN(Content Delivery Network)

``` bash
Ref 
- https://stackoverflow.com/questions/8779197/linking-files-directly-from-github   
```

Because default raw url of github repository may limite file size and bandwidth so to use it as CDN you need some trick shown below,

1. Get the raw url of file from github. For example,
`https://raw.githubusercontent.com/Napat/git_newbie/master/README.md`

2. There are many options to generate the url,

    2.1 Visit `https://gitcdn.link/` This link will auto get/update the latest version of files and no damage from high traffic volumes.
    output: `https://gitcdn.link/repo/Napat/git_newbie/master/README.md`

    2.2 Visit `https://min.gitcdn.link/` This link give you the option of auto minifying your html, css and javascript.
    output: `https://min.gitcdn.link/repo/Napat/git_newbie/master/README.md`

    2.3 Visit `https://rawgit.com/`  
    output:  
    production `https://cdn.rawgit.com/Napat/git_newbie/master/README.md`  
    development `https://rawgit.com/Napat/git_newbie/master/README.md`  

You can use output link in many situations. For example,

- Create your own CDN.
- Direct link to any web library such as js, css that available in github without donwload to your workspace.

--------------------------------------------------------------
  
## Update change at upstream repository to your own github forked repository  
  
[[Scenario]]  
A few day later after forked github project to your github account and cloned to your local machine, another change was made at origin project.
Now, you want to apply that change to your github repository.  
  
``` bash
# 1. Add upstream repo
git remote add upstream https://github.com/whosupstream/whatproject.git

# 2. Fetch all branches of upstream into tracking branches
git fetch upstream

# 3. Go to your main branch that you want to apply the change
git checkout master

# 4. Rebase main branch with upstream 
git rebase upstream/master

# 5. Push to your github project
# You may need to unprotected master branches in some git's server.
# Github Project: Settings > Branches > See "Protexted branches" section
git push origin master

```  

--------------------------------------------------------------
  
## Embed YouTube Videos to Github markdown  

They can't be added directly but can add an image with a link to the video like this:

``` bash
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE
" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>
```

<a href="http://www.youtube.com/watch?feature=player_embedded&v=2MsN8gpT6jY
" target="_blank"><img src="http://img.youtube.com/vi/2MsN8gpT6jY/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>
  
Or, in pure Markdown, but losing the image sizing and border:  

``` bash
[![IMAGE ALT TEXT](http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](http://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE "Video Title")
```
[![IMAGE ALT TEXT](http://img.youtube.com/vi/3jt-J4Rc__M/0.jpg)](http://www.youtube.com/watch?v=3jt-J4Rc__M "Github page")

--------------------------------------------------------------
  
## Checklist in markdown documents

- [x] List01: feature 001
- [x] List02: feature 002
- [x] List03: feature 003
- [x] List04: feature 004
- [ ] List05: issue 001
- [ ] List06: feature 005
  
--------------------------------------------------------------  
  
## git submobule remove cache issue

``` text
Ref:
- https://stackoverflow.com/a/35205393
- https://stackoverflow.com/a/1789374
```

TL;DR,  
If you want is to put your submodule into the main repository.
you have to remove the submodule and re-add the files into the main repo.
and you have to delete reference to submodule HEAD with:

``` bash
git rm --cached <submodule_dir>
```

--------------------------------------------------------------

## Golang: Working with private repository(ie. local gitlab)

One shot cmd to prove of concept: `env GIT_TERMINAL_PROMPT=1 go get -insecure  gitlab.myprivate.io/napat/mylogger`

### .netrc to login without prompt

### github

``` text
Ref:
https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line
```

### gitlab

1. Create **personal_access_tokens** with Scopes at [access tokens profile setting](https://gitlab.myprivate.io/profile/personal_access_tokens)
2. `echo "machine gitlab.forthrd.io login [your username] password [token from step 1.]" > ~/.netrc`
3. Try git clone(https) or `go get` without `GIT_TERMINAL_PROMPT=1`

### encrypt .netrc to .netrc.gpg

https://jano.com.es/git/https/2018/10/02/automate-git-authentication-with-http.html

--------------------------------------------------------------
