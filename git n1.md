#### Git

    it is a free and open source distributed version control system  designed to handle everything from small to very large projects with speed and efficiency.


    it helps to see the entire chnages,progression of any project in one place.

    Git is easy to learn.

#### Git Repository

    A Git repository is the .git/ folder inside a project. This repository tracks all changes made to files in project, building a history over time.

#### Git Commands

#### git init
    
    This command turns a directory into an empty Git repository. This is the first step in creating a repository. After running git init, adding and committing files/directories is possible.

     $ cd /file/path/to/code
    # make directory a git repository
     $ git init

    Ex: c:\user\cp nm/doc/fd nm/notes/git init
     
     Intialized empty git repository
     
#### git add

     Adds files in the to the Git. Before a file is available to commit to a repository, the file needs to be added to the Git index. There are a few different ways to use git add, by adding entire directories, some files.

     $ git add<file or directory name> 

     ex: c:\user\cp nm/doc/fd nm/notes/git add .

#### git commit

    Record the changes made to the files to a local repository. For easy reference, each commit has a unique ID.

    to include a message with each commit explaining the changes made in a commit. Adding a commit message helps to find a particular change or understanding the changes.

    it is used to saves the snapshot to the file.

    $ git commit -m "commit msg"

    ex: c:\user\cp nm/doc/fd nm/notes/git commit -m "my first commit"

#### git push

    Sends local commits to the remote repository.

    it is used  to move the file to main folder.    

    $ git push

    ex: c:\user\cp nm/doc/fd nm/notes/git push

#### git pull

    To get the latest version of a repository run git pull. This pulls the changes from the remote repository to the local computer.

    $ git pull
    
    ex: c:\user\cp nm/doc/fd nm/notes/git pull

#### git config
    
    git config is how to assign these settings. Two important settings are user user.name and user.email.

    With git config, a --global flag is used to write the settings to all repositories on a computer. Without a --global flag settings will only apply to the current repository that you are currently in.

    $ git config --global user.email "email.id@add.com"
    $ git config --global user.name "name"

    ex: c:\user\cp nm/doc/fd nm/notes/git config --global user.email "prathyusha@gamil.com"
    c:\user\cp nm/doc/fd nm/notes/git config --global user.name"chinni"

#### git status

    git status will return the current working branch. 
    
    If a file is in the staging area, but not committed, it shows with git status. Or, if there are no changes it’ll return nothing to commit, working directory clean.

    $ git status

    ex:c:\user\cp nm/doc/fd nm/notes/git status

#### git log

    it is used to show total history of file.

    $ git log
    
    ex:c:\user\cp nm/doc/fd nm/notes/git log
