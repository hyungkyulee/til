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

#### check the history log and try to 1-step back from the commit history
```bash
$ git log
$ git reset --soft HEAD~1
```

#### Hard reset
```
git reset --mixed "커밋 id"
```

#### Hard reset
```
git reset --hard "커밋 id"
```

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

