# **Shell Scripting**

Shell is how we can interact with the hardware without dealing with the kernel. 

Layers:
1. Hardware 
2. OS (Kernel) - delicate and shouldn't be messed with
3. Shell - wraps around OS and protects it
4. Terminal - how we interact with Shell
5. User

## **Traversy Media**

### **Boilerplate**
Always start each bash with a PATH to the bash program. 

`#! /usr/bin/bash`

### **Comments**
`#comments are just like Python`

### **Commands**
List and perform your normal commands such as 

```bash
cd dir1
date 
cal 
which bash
```

### **Variables**
No spaces for assignment!
```bash
name='Bob'
echo "Hello my name is $name"
```

### **User Input**
Use `read` to grab input with `-p` for **prompt**. Automatically assigns the variable to `NAME` which can be used later.  

```bash
# Will set prompt next to the echo text
read -p "Enter your name: " NAME
echo "Hello $NAME, nice to meet you!"

# Or will set propt after the echo test on next line
echo 'Enter your name:'
read NAME
echo "Hello $NAME, nice to meet you!"

```

### **if-elif-else**
`end` is `fi` of `if` backwards. 
```bash
if [ "$NAME" == "Brad" ]
then
  echo "Your name is Brad"
elif [ "$NAME" == "Jack" ]
then  
  echo "Your name is Jack"
else 
  echo "Your name is NOT Brad or Jack"
fi
```

### **Comparisons**
* val1 -eq val2 Returns true if the values are equal
* val1 -ne val2 Returns true if the values are not equal
* val1 -gt val2 Returns true if val1 is greater than val2
* val1 -ge val2 Returns true if val1 is greater than or equal to val2
* val1 -lt val2 Returns true if val1 is less than val2
* val1 -le val2 Returns true if val1 is less than or equal to val2

```bash
NUM1=31
NUM2=5
if [ "$NUM1" -gt "$NUM2" ]
then
  echo "$NUM1 is greater than $NUM2"
else
  echo "$NUM1 is less than $NUM2"
fi
```

### **Conditions**
* -d file   True if the file is a directory
* -e file   True if the file exists (note that this is not particularly portable, thus -f is generally used)
* -f file   True if the provided string is a file
* -g file   True if the group id is set on a file
* -r file   True if the file is readable
* -s file   True if the file has a non-zero size
* -u    True if the user id is set on a file
* -w    True if the file is writable
* -x    True if the file is an executable

```bash
FILE="test.txt"
if [ -e "$FILE" ]
then
  echo "$FILE exists"
else
  echo "$FILE does NOT exist"
fi
```
### **Loops**
```bash
NAMES="Brad Kevin Alice Mark"
for NAME in $NAMES
  do
    echo "Hello $NAME"
done
```
A ***Great*** application for this is renaming a slew of files.
```bash
FILES=$(ls *.txt)
NEW="new"
for FILE in $FILES  
  do
    echo "Renaming $FILE to new-$FILE"
    mv $FILE $NEW-$FILE
done
```

### **Functions**

```bash
#Without parameters
function sayHello() {
    echo "Hello World"
}

sayHello

#With parameters. They are positional parameters and must be labeled as $1, $2, $3, etc.
function greet() {
    echo "Hello, I am $1 and I am $2 years old. Is your name $1 too?"
}

greet "Kyle" "35"
```


