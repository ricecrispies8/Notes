# **Git Basics**
## **What is Git?**
Git takes a snapshot of each file after commit (save) and saves a reference to that snapshot. If a file has not been altered it points to the last unchanged file, but still uses that same file version. In 'Version 3' File B is still in the original state, but is still brought along. 
![image](./images/git_storage.png "From Git-scm book")

`$git init` -> create new repository in current directory. Creates .git file with everything you need to work with git

`$git status` -> to check status of repository including for the current branch, what commits have been done, and what files are or are not being tracked.