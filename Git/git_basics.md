# **Git Basics**
## **What is Git?**
Git takes a snapshot of each file after commit (save) and saves a reference to that snapshot. If a file has not been altered it points to the last unchanged file, but still uses that same file version. In 'Version 3' File B is still in the original state, but is still brought along. 

![image](./images/git_storage.png "From Git-scm book")

Everything is committed (saved) locally and isn't pushed into Github unless distinctly 'pushed.'


>Git has three main states that your files can reside in: modified, staged, and committed:
>* Modified means that you have changed the file but have not committed it to your database yet.
>* Staged means that you have marked a modified file in its current version to go into your next commit snapshot.
>* Committed means that the data is safely stored in your local database.

![image](./images/stages.png)

>* The working tree is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.
>* The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit. Its technical name in Git parlance is the “index”, but the phrase “staging area” works just as well.
>* The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you clone a repository from another computer.

>The basic Git workflow goes something like this:
>1. You modify files in your working tree.
>2. You selectively stage just those changes you want to be part of your next commit, which adds only those changes to the staging area.
>3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.


## **Creating a new Github repository**
1. Create repository on Github.com
2. `$git clone git@github.com:<user_name>/<repo_name>.git`
3. Move already created files or whatnot into the cloned folder.
4. `$git add *`
5. `$git commit -m "commit name"`
6. `$git push -u origin main`

## **Recording Changes to a Repository**
All files are either tracked or untracked. Tracked means Git knows about it and has an existing snapshot of it. Git also knows if those files have been modified. To start tracking a file (use `$git status` to see which files are un/tracked), simply using `$git add filename.ext`.

You can create a list of files/folders to ignore using .gitignore file. Then list all files/folders using regex-style commands. 

If you move or rename a file/folder, this also needs to be recorded in Git as Git does not track file movement/renaming. This can be done quickly with `$git mv file_from file_to`. Otherwise you will need to remove/rename the file normally, drop it from Git and then readd it again. 

## **Basic Commands**
`$git init` -> create new repository in current directory. Creates .git file with everything you need to work with git. Not quite sure how to move this over to Github.

`$git status` -> to check status of repository including for the current branch, what commits have been done, and what files are or are not being tracked. Use `-s` tag to make the status less verbose. 

`$git config --list` -> to check the username, email, and default branch name

`$git add *` -> adds all files or folders in the git folder to the staging area. 

`$git commit -m "commit name"` -> commits (saves) all those files in the staging area. Can skip the staging area altogether by using the `-a` tag. 

`$git rm filename` -> removes file from being tracked. 

`$git mv file_from file_to`-> If you move or rename a file/folder, this also needs to be recorded in Git as Git does not track file movement/renaming. Use this method, otherwise you will need to remove/rename the file normally, drop it from Git, and then read it again. 

`$git push -u origin main` ->pushes your commits to the main file on Github. If you are not using SSH, you will need your username and password which is apparently depracated. Guess you better have SSH ready. 

`$git clone git@github.com:<user_name>/<repo_name>.git`-> grabs a copy of an already existing repo on Github using SSH. Since SSH is already set-up on this computer, everything is good. 
