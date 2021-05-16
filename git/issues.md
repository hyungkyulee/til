# Git : Common error and Solution

## Refusing to merge
[Symptom] 
fatal: refusing to merge unrelated histories

[Solution]
``` git pull origin master --allow-unrelated-histories ```
  
## nothing added to commit but untracked

[Symptom]
```
$ git commit -am "first commit"
On branch master

Initial commit

Untracked files:
        .gitignore
        .gitmodules
        LICENCE
        README.md
        doc/
        src/
        tools/

nothing added to commit but untracked files present
```

[Solution]
The new files and changes are added to git
```
git add -A
```

## Reject to push

[Symptom]
```
$ git push -u origin master
Warning: Permanently added the RSA host key for IP address '140.82.118.3' to the list of known hosts.
Enter passphrase for key '/c/Users/hyungkyu.lee/.ssh/id_rsa': 
To github.com:hyungkyulee/hbbtvReference.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:hyungkyulee/hbbtvReference.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

[Solution]
There are some changes on the server, but no pull
```
git pull
```

## no tracking information when pull

[Symptom]
```
$ git pull
Enter passphrase for key '/c/Users/hyungkyu.lee/.ssh/id_rsa': 
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> master
```

[Solution]
Set upstream or remote origin
```
$ git branch --set-upstream-to=origin/master master
Branch 'master' set up to track remote branch 'master' from 'origin'.
$ git push --set-upstream origin master
```
OR
```
$ git branch --set-upstream-to=origin/master master
$ git pull origin master
```

## refusing to merge unrelated histories

[Symptom]
```
$ git pull
Enter passphrase for key '/c/Users/hyungkyu.lee/.ssh/id_rsa': 
fatal: refusing to merge unrelated histories
```

[Solution]
```
$ git branch --set-upstream-to=origin/master master
$ git pull --allow-unrelated-histories
```

## merge failed

[Symptom]
```
$ git pull
CONFLICT (add/add): Merge conflict in README.md
Auto-merging README.md
Automatic merge failed; fix conflicts and then commit the result.
```

[Solution]
1) merge manually opening the file
2) re-run the git add and commit
3) git push


