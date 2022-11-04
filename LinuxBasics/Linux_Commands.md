# Linux Command Line Basics
*Note: You can view this at anytime using the following command in the terminal*
`$grip -b Linux_Commands.md` (`-b` is for browser)

Also, use *ctrl-shift-v* to view markdown as splitscreen.

## **Keyboard Shortcuts**
**ctrl + a** -> acts like **HOME** key

**ctrl + e** -> acts like **END** key

**alt + f** -> moves one word forward

**alt + b** -> moves one word backward

**alt + u** -> makes entire word uppercase

**alt + l** -> makes entire word lowercase

**ctrl + k** -> cuts from prompt to end

**ctrl + u** -> cuts from prompt to beginning

**ctrl + y** -> pastes it back

**ctrl + l** -> same as `$clear`



## **Change directory**

`$cd dir_name` -> changes directory

Use **TAB** key to autofill after typing a few letters for a quicker way of using `cd`. Use **TAB** twice will open up suggestions. 

## **Listing (*ls*)**
`~` -> is home directory (```$ls ~``` shows all list items in home)

`/` -> is root directory

`..` -> goes up one layer (parent directory)

`pwd` -> prints working directory

`-` -> will take you to previous directory (does not work with `ls`)

`$ls /home/Kyle/Desktop` -> gives *ls* of absolute file path. Can also look into folder of whatever current directory is. Gives in alphabetical order

```bash
$ls -> dir1, dir2, dir3
$ls dir3 -> files inside dir3
```

`$ls -i` -> shows *inode* of all files (inode # is an unique id)

`$ls -l` -> *long* version. Shows file size in bytes, ownder, permissions, modification timestamp, etc

`$ls -a` -> shows *all* files including hidden files (.hidden). There is also an alias `$la` which will do this automatically.

`$ls -t` -> shows files in according to *time* (newest to oldest) order rather than alphabetical

`$ls -r` -> *reverses* order

`$ls -R` -> *Recursively* shows all `ls` on all children/descendant directories.

`$ls -i -l` -> shows a concatenated version of both (`$ls -il` also works). Could also pair `$ls -tr` to show oldest file first.

`$ls --` -> and then hitting **tab** twice will give all the possible `-x` commands

`$ls *.txt` -> shows all files ending in .txt.

## **Hardlinks vs Softlinks**

Each file has an inode (index node) which contains metadata of the file including inode #, file size, file type, permissions, etc.

### **Hardlinks**
HardLink == copy of original file with a different name but same inode #, file size, etc. Deleting original file will not change hardlinks. Cannot create new directory. 

`$ls filename hardlink1` -> creates hardlink of 'filename'. Has same inode #.

### **Softlinks**
Softlink == shortcut in windows. Original file has an inode # and its soflink files (shortcuts) have *different* inode #s. If original is deleted softlinks will be useless. Softlinks are smaller as well. Can be used on directories as well. 

`$ls -s filename softlink1` -> creates softlink of 'filename'. Has different inode #. Will have different file size when looking as `$ls -l`. 


## **Working with files/folders**
### **Creating**
`$touch filename` -> changes modification timestamp of existing file or create a new file. Place extension at end of filename otherwise it will be a .txt file. Use quotes for file names with spaces `'file name'`. Can use an escape `\*space*` as well

`$mkdir folder1` -> *makes directory*. Use quotes for file names with spaces `'dir name'`

Use the `\` escape to use $ > < & | ; " ' \ for a file name. `\&filename` -> `&filename`

### **Deleting**
`$rmdir folder1` -> *remove* empty *directories* only. Use quotes for file names with spaces `'file name'`.

`rm file1` -> *removes* file. Use quotes for file names with spaces `'file name'`.

`rm -R folder1 file1` -> can delete files or any type of directory

`rm -Ri folder1` -> Recursively goes through each directory and asks if you want to delete that specific folder or file. `-i` stands for *interactive*

`rm -f file1` -> removes everthing listed *forcefully*

`rm -v file1` -> *verbose* removal

### **Copying**
`$cp file1 file2` -> *copy* file1 as new file file2. Can update an existing file as file1 if file2 already existed. Use quotes for file names with spaces `'file name'`.

`$cp file1 file2 dir1` -> *copies* the files into the destination directory

`$cp -R dir1 dir2` -> *recursively copies* dir1 into dir2

Can use `-i` option to *interactive* prompt if you want to override or do specific things while copying. `-v` gives a *verbose* summary of what it did.

### **Moving and Renaming**
`$mv file1 cheeseburger` -> *moves* file1 to be renamed to cheeseburger. Same goes for directories. Use quotes for file names with spaces `'file name'`.

`$mv file1 .cheeseburger` -> turns file1 into a hidden file

`$mv file1 file2 dir1` -> moves all files into dir1

`$mv dir1 dir2` -> moves dir1 into dir2. If dir2 does not exist, it will rename dir1 as dir2. 

`$mv dir1 dir2/other_dir`-> moves dir1 into a new directory destination inside dir2. 

Can use `-i` option to *interactive* prompt if you want to overwrite or do specific things while copying. `-v` gives a *verbose* summary of what it did.

### **File extensions**
filename.txt, filename.pdf, filename.jpg, etc are meaningless in linux. They are just a naming convention. 

`$file banana.jpeg` -> gives a *file* description 

```bash 
banana.jpeg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, baseline, precision 8, 257x196, components 3
```
## **Viewing and Editing Files**
### **gedit**
`$gedit filename` -> creates or opens new text file depending on it if already exists.

### **nano**
`$nano filename` -> does the same as `gedit` but does the editor in the terminal. 

**ctrl + w** -> is a search (only gives first result)

**ctrl + j** -> justifies current line

**ctrl + o** -> `write out` is the same as `save`

### **less**
`$less filename` -> provides a read only format for opening a file. Use **q** to quit. 

### **cat**
`$cat filename` -> provides a read only format for opening a file without taking you out of the terminal

`$cat filename1 filename2` -> concatenates two files together in the terminal

### **tac**
`$tac filename` -> exactly the same as **cat** but with reverse order **cat -> tac** by line

### **History**
`$history` -> shows entire history. Adding a number next to it will show the last **x** commands. Use `$!2367` to use command 2367 from history without having to retype it. 

`$gedit ~/.bash_history` -> opens a text file containing all the history commands

`$history -c` -> clears entire history

### **head and tail**
`$head hundred` -> shows first 10 lines by default

`$head -n 20 hundred` -> shows first 20 lines

`$tail -n 20 hundred` -> shows last 20 lines instead

### **wc**
`$wc filename` -> **word count**. Outputs # lines, # words, # bytes/character numbers

`$wc -L filename` -> prints number of characters in longest line

## **Commands**
Use `$type command_name` to identify the type of command it is. 

4 types:
1. Executable Programs - does something like a function. It grabs these programs from the **bin**.

    `$type cp` -> `cp is /usr/bin/cp`
    `$file /bin/cp` -> has work **executable** in the long description

2. Shell Builtins - commands which are in the terminal.

    `$type cd` -> `cd is a shell builtin`

3. Shell Scripts - file containing a command (will go over these later). Also located in the **bin**.

    `$type bzdiff` -> `bzdiff is /usr/bin/bzdiff`
    `$file /bin/bzdiff` -> `bzdiff: POSIX shell script, ASCII text executable`

4. Alias - commands you build yourself. 

    `$ls` -> `ls is aliased to 'ls --color=auto'`

### **which**
Shows location of the executable file (all commands ***except*** shell builtins)

`$which ls` -> `/usr/bin/ls`

### **help and man**

`$help cd` -> gives **help** options that come with shell builtin commands

`$man less` -> the **manual** for executable commands

### **whatis**

`$whatis cp` -> explains **what is** a command's basic function is. Does not work with shell builtins

## **Making your own commands**

### **Combining multiple commands**
1. `$touch text.txt; mv text.txt dir1` -> Separate commands with a semi-colon and performs them in order. Creates `text.txt` file and moves it instantly to `dir1` directory. Will ignore an incorrect command and execute other commands. Can also use `exit` to close terminal once done with other commands. It will `exit` instantly and ignore other commands which come after. 
2. `$touch text.txt && mv text.txt dir1` -> same as with semi-colon except will ignore all commands after an error. AKA a "short-circuit" evaluation. 

### **Wildcards**
#### * **and ?**
`*` -> works just like with SQL. Represents one or more characters. 

`$mv * dir1` -> moves all files to `dir1`

`$mv *.txt dir1` -> moves all `.txt` files to `dir1`

`$mv n*e dir1` -> moves all files that start with ***n*** and end with ***e***

`?` -> represents single character. Can use multiple to represent the numbers of chars needed.

`$mv file?.txt dir1` -> will move `file1.txt`, `file2.txt`, but not `file53.txt`

#### **Other Wildcards**

`$mv [abc]* dir1` -> will grab any file starting with ***a, b*** or ***c***

`$mv [!abc]* dir1` -> opposite (complement) of above

`$mv [0-9]* dir1` -> to grab a range (includes 0 and 9)

`$mv [[:upper:]]* dir1` -> grabs all starting with uppercase (`[[:lower:]]` works too of course)

`$mv *[[:digit:]] dir1` -> all files that end with a digit.

`$mv *[[:alpha:]] dir1` -> all files that end with a letter.

`$mv *[[:alnum:]] dir1` -> all files that end with a letter or digit.

Of course we can combine some as well:

`$mv *[[:alpha:]]*[[:digit]].??? dir1` -> starts with a letter, ends with a digit, and has a three character extension.

### **Alias**
Alias gives ability to make your own commands. First we must check if the system alrady has the name we want to use. `$type new_command_name`. This is only temporary and will be gone as soon as you close the terminal.

`$alias new_command_name="cd Desktop;mkdir dir1;touch dir1/file1"` -> will perform the command inside the double quotes. Notice there are no spaces once the alias is named. 

`$unalias new_command_name` -> deletes alias

To make a permanent alias:

`$gedit ~/.bashrc`

Navigate to `# **USER CREATED aliases**` (line 98) and add permanent aliases there. `learn` is there for reference.

`$alias` -> will show all permanent aliases saved in the .bashrc file.