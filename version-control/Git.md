# Git commands

#### Managing
* git init , creating empty repository
* git clone <repository­url> , copy repository from remote repository url
* git add , add files to commit
* git add * , add everything
* git commit ­m "Commit message" , commit
* git push <remotename> <branchname> , ex: git push bitbucket feature/1_example
* git status , local status
* git remote add origin <remote­repository­url> ex: git remote add origin https://github...
* git remote ­-v , list remote repositories
* git remote show <remotename> , list all remote branches
* git checkout <branchname>, switch to other local branch
* git checkout ­b <branchname> , create new branch from active branch
* git branch , list local branches
* git branch ­d <branchname> , delete local branch
* git push <remotename> :<branchname> , delete a branch on remote repository
* git push <remotename> ­­delete <branchname> , the same as above
* git pull , fetch and merge from remote to local
* git merge <branchname> , merge a choosen branch into active branch
* git diff , merge conflicts
* git fetch <remotename> , get all remote branches to local remote
* git fetch <remotename> <branchname> , get one branch from remote and update local
(remotename/branchname)
* git fetch <remotename> <branchname>:<branchname> , get one branch from remote,
* git merge <remotename>/<branchname> , merge a remote local copy to active branch
* git checkout ­­ <filename> , undo changes in the file before commit
* git reset ­­hard origin/master , undo all changes, even after commit and return to the state
of remote branch
* git branch ­m <oldname> <newname> , rename any local branch
* git branch ­m <newname> , rename current (active) branch
* git rebase <remotename>/<branchname> , after git fetch <remotename> <branchname>
* git log,  - list of commits (with commit_sha)

#### Reset
* git merge --abort, for new git version, undo last merge (if merge was succesful - MERGE_HEAD exists)
* git reset --hard commit_sha, reset to given commit
* git reset --merge ORIG_HEAD,  undo last merge
* git reset --hard origin/master, reset to some remote tracking branch
