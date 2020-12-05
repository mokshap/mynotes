## Git Notes

### To create a new project
    mkdir mypoject
    cd myproject
    git init

### To clone a existing project into the current directory
    git clone https://github.com/ekanna/myproject

### To clone a existing project into a specified directory
    git clone https://github.com/ekanna/myproject mypath/myproject.git

### To untrack specified file or directories
    echo 'myfile' >> .gitignore

### To track files
    git add filename // To stage specific file
    git add . // To stage all new and modified files
    git add -U // To stage, modified and deleted files
    git add -A // To stage all changes

### To commit the changes
    git commit -m 'commit message'
    
    // Shortcut for 
    #git add modifiedFile
    #git commit -m 'commit message'
    
    git commit -am 'commit message'

### To push to remote server
    git push

## Tag
Tag is a permanent pointer for a specific commit

    git tag 1.0 

## Update git password
It is a two step process

    git config --global credential.helper store
    git pull  //Now this command will ask for password

## Branches
Branch is a pointer to specific commit.
Branch is a moving pointer to series of commits in the repository.
Whenever there is a new commit in the branch, pointer will move to new commit.

Firt commit is the root of the tree. Master is the default pointer to it.

### List branches
    git branch // list of branches higlighing active branch with * 

### Working with branches
    git branch kanna // creates a new branch kanna
    git checkout kanna // moves HEADER to kanna from the master (assuming one is in master branch)
    
    git checkout -b kanna // short cut for above two steps
    
    git checkout master // moves HEAD to master
    
    git merge kanna // merges the commits made in the kanna branch into master
    
    // deletes branch
    git branch -d <branch_name>
    
    // deletes a remote branch
    git push -d origin <branch_name>
    
### Remote Branches // TODO
    git remote add origin remoteURL
    git push origin mybranch
    
    origin/myBranch is the remote branch

### List Remote urls
    git remote -v

### To overwrite remote urls use set-url:
    git remote set-url origin git://new.url.here
    
### Orphan Branch
    git checkout --orphan branchName
    // Orphan branch can't be created with "git branch" command
    // Purpose of creating orphan branch is to create new branch without commit history
    // And start making commits in it as if that is the start of the commit history
    
### worktree branch
    git worktree add myworktree // creates myworktree folder and myworktree branch
    
    
### Git Revert
    // list of commits with refs
    git reflog
    
    // reverts latest 2 commits without asking for commit messages
    git revert --no-commit HEAD@{2}..HEAD
    
    // now commit 
    git commit -m 'reverted to old commit'
    
    
### Reset Vs. Revert // TODO
    git reset // use for local branches as it erases history
    git reset HEAD~2 // moves the head to specified point and erases history
    
    git revert // use for remote branches. It reverts specifi commit(s)
    git revert sha1ID // for reverting single commit
    git revert sha1ID1 sha1ID2 // for reverting multiple commits
    
    git revert is safest option as it creates new commit without touching history.
    
### Rebase Vs. Cherry-pick // TODO
    git rebase 

## Submodules
Useful for third party modules like modules that goes into 'vendor' directory in golang.
Warning: Don't make any changes in submodules repositories directly! to avoid unnecessay conflicts.

    git submodule add urlpath destintionFolder
    git submodule add https://github.com/ekanna/SubProject.git vendor/github.com/ekanna/SubProject

Also useful for statci site generators like Hugo 
where you can have source files in one repo and generated content in another repo and link them using submodule

    git submodule add https://github.com/ekanna/ekanna.com.git public
    cd public
    // commit changes
    // Here we will commit changes to submodule folder itself directly 
    // because it is auto genereated unlike vendor folder above

### Clone a repository along with submodules
    git clone --recursive https://github.com/ekanna/MainProject

### Pulling in Upstream changes
    git submodule update --remote vendor/github.com/ekanna/SubProject
    
## Worktrees
To convert branch into directory/folder

Useful for simultaneously comparing the behavior (not source) of different versions.
For example different versions of a web site or just a web page.

    git worktree add folder branch
    git worktree add v1 version1
    
    
    git worktree list
    git worktree add folder // converts current branch upto latest commit into directory and also creates a newBranch
    git worktree add folder mybranch // converts specified branch into directory
    git worktree add folder -b newBranch // creates directory and branch with different names
    git worktree add folder -b newBranch mybranch

    // It creates a new directory/tree named "kanna" and also a new branch named "kanna" based on current HEAD
    // To go to this newly created branch use "cd kanna/"
    git worktree add kanna
    
    // It creates a new directory/tree named "koti" and attach "mybranch" to it. It won't create a new branch named koti.
    // Now mybranch can be navigated simply "cd koti/" and HEAD is pointed to mybranch.
    git worktree add koti mybranch
    
    // It creates worktree with directory named chinna and a new branch with the name mybranch77 instead of default branch name chinna.
    git worktree add chinna -b mybranch77
    
    // It throws an error as mybranch is already attached to koti worktree above.
    // This is to prevent accidentlly creating new worktree based on the same branch.
    git worktree add chinna2 mybranch
    
    // It will create a new worktree based on mybranch, eventhough mybranch already attached to koti worktree. 
    It basically overcomes the above limitation
    git worktree add chinna2 -b chinna2 mybranch 

### List worktrees
    git worktree list

### Clean worktree
    rm -rf folder
    git worktree prune
    rm -rf .git/worktrees/folder/
    
### worktree use case example
https://gohugo.io/tutorials/github-pages-blog/


## Submodules vs Worktree
    submodules are meant for linking external repos
    worktrees are meant for viewing branches as directories

### TimeStamps
    when you clone a project, all files will have timestamps of cloning time in local machine
    They won't get remote reposity file timestamps.
    If you wan't sync files to remote service like AWS-S3 which syncs based on created and modified timestamps 
    and file sizes. This can lead to problems like uploading same files again and again.
    To overcome this, one may use below bash script OR etags
    
### Syncing data based on changes in data to remote AWS S3 for satic site generators like Hugo
* https://www.lambrospetrou.com/articles/aws-s3-sync-git-status/
```bash
#!/bin/bash
set -ex

FILES=()
for i in $( git status -s | sed 's/\s*[a-zA-Z?]\+ \(.*\)/\1/' ); do
    FILES+=( "$i" )
done
#echo "${FILES[@]}"

CMDS=()
for i in "${FILES[@]}"; do
    CMDS+=("--include=$i""*")
done
#echo "${CMDS[@]}"

echo "${CMDS[@]}" | xargs aws s3 sync . s3://www.lambrospetrou.com --dryrun --delete --exclude "*" 
```
The above code is more like hack.
Proper way todo this is to create etag(EntityTag) for all local files using crypto/md5
AWS S3 automatically creates etags for uploaded files. 
Compare localetags with awsetags identify New/Modified/Deleted files
And sync them using aws-sdk

### Workflow when you submit a Pull request
```
// Create a new branch out of master
git checkout -b myBranch

// make some changes and do commit

git add .
git commit -m ‘kkkk’

// make some more changes and commit

git add .
git commit -m ‘kkkk’

// Now rebase against master branch to merge commits as single commit

git rebase -i master

Now pick top most commit and squash rest
And add final commit message

And send a pull request.

Repo Main Author does below:
// Review
git fetch myBranch
git checkout myBranch
git status
git diff

// Merge
git checkout master
git merge myBranch

// short hand for fetch and merge commands
git pull myBranch
```

### To download a single file from git

#### Using git and ssh
```
git archive --remote=ssh://bitbucket.org/ekanna/books2reports.git master README.md | tar -x
```

#### Using wget and password
```
wget --user=ekanna --ask-password https://bitbucket.org/ekanna/books2reports/raw/master/Dockerfile
OR 
wget --user=ekanna --password=SUPERSECRET https://bitbucket.org/ekanna/books2reports/raw/master/Dockerfile
```
Notice ```src``` replaced with ```raw``` in the path

### git url with and without password
```
git clone https://username:password@github.com/username/repository.git
git clone https://username@github.com/username/repository.git
```
It will prompt you for your password.
