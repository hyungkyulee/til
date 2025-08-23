# Git commands
## Init Commands on a new repo creation
…or create a new repository on the command line
- echo "# git" >> README.md
- git init
- git add README.md
- git commit -m "first commit"
- git branch -M master
- git remote add origin git@github.com:hyungkyulee/git.git
- git push -u origin master
***
…or push an existing repository from the command line
- git remote add origin git@github.com:hyungkyulee/git.git
- git branch -M master
- git push -u origin master

* since 2020, master branch has been changed to main.

***
OR

git brach -m master main
git fetch origin
git branch -u origin/main main
git remote set-head origin -a

## Basic Commands

***
### Status check 
```
git status
```

***
### Branch Handle
#### Create a new branch and move to the branch
```
git checkout -b [a new branch name]
```

#### Show the branches
```
git branch
git branch -a (show all of branches including remote branch)
```

#### Work on the new branches
```
git add -A
git commit -am "barcode Saga updated"
git status
git push origin barcode -u
then,
git commit -am "comment1"
git push (* don't need to -u option once it's set to origin)
```

#### Change to another branch or master
```
git checkout [other branch name] (or master)
```

#### checkout to the remote branch
```
git checkout --track origin/[branch name]
```

#### set to remote branch
```
git checkout --track origin/[branch name]
git pull
```

#### Update master and apply the new master to the working branches.
```
git pull (at master)
git checkout [branches]
git merge master
```
> 'git pull master' won't work. git pull = git fetch + git merge, but only work in the same branch.

[rebase]
```
git fetch // local sync with remote, or you can go to master and git pull first before doing a rebase
git pull --rebase origin master

// push forcely if no conflict
git push -f

// resolve them if there is conflicts
// and git add / commit to apply for the resolution of the conflicts

// continue a rebase until no-conflict
$ git rebase --continue
Successfully rebased and updated refs/heads/74719-exclude-hot-slice-pizza.

```

#### Delete Branch
```
git branch -d [branch name]
```

***
### change git commit
```
git commit --amend -m "[new message]"
git push
```

***
### Reset
(* source: https://devconnected.com/how-to-remove-files-from-git-commit/)

#### Soft reset check the history log and try to 1-step back from the commit history
Move to the previous commit, but index remains on history (staged + local file remains)
```bash
$ git log
$ git reset --soft HEAD~1
```

Move to the previous commit, and index deleted (unstaged + local file remains)
#### Mixed reset
```
git reset --mixed "커밋 id"
```

Move to the previous commit, and local changed deleted
#### Hard reset
```
git reset --hard "커밋 id"

or

git reset --mixed HEAD~1
git checkout .
```
> hard reset = mixed reset + git checkout .
> comparison of soft/mixed/hard reset : https://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard

#### remove .git related files
```
rm -rf .git*
```

#### remove committed files
```
git rm --cached <file name>
git status (we can see the removed files)
rm -rf <file name>
git add -A
git commit -am "xxx"
git push
```

#### gitignore
if a file is already commited to remote, it won't be affected by gitignore. 
```
// e.g. .env file to ignore
git rm --cached .env
```

