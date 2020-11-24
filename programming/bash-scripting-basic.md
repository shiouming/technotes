# Quick notes - Bash shell scripting basic

Creation date: 2008-6-19

Tags: *bash, linux*

---


Having following line at the first line of script file will tell system which program should be use 
to run the script file. In this case, **Bash**.
```
#!/bin/bash
```

General stuff
```
# Exit the shell with status n
exit [n]

# To exit from a function
return [value]

# To import other source file, similar to #include in C
. <another-source-file>

# Turning off and on the screen echo, e.g. while user entering password.
stty -echo
stty echo

# Saving and restoring stty settings
old_tty_settings=$(stty -g)
stty "$old_tty_settings"
```

Redirection
```
# Redirect output of command into a file called file.log, either by overwriting or by appending the 
#   file's contents.
command > file.log   # as new file, or overwrite if file.log already exists.
command >> file.log  # as new file, or append if file.log already exists.

# Redirect the stdout or stderr into file.log
command 1>> file.log
command 2>> file.log

# Redirect both the stdout and stderr to file.log
command >> file.log 2>&1

# Redirect the stderr to null device (silent the stderr)
command 2>> /dev/null
```

Getting user input
```
# Reading user input from prompt
read NAME
echo "Hi $NAME!"

# Display a menu to get user input with 'select' statement
OPTIONS="Hello Quit"
select opt in $OPTIONS; do
    if [ "$opt" = "Quit" ]; then
        echo done
        exit
    elif [ "$opt" = "Hell" ]; then
        echo Hello World
    else
        clear
        echo bad option
    fi
done
```

Variables
```
# Variables in Bash shell can contain string or numbers, and does not require explicit declaration. 
#   When referring the value stored in variable, need to prefix variable name with $
MYSTRING="First string!"
echo $MYSTRING

# We can have local variable within a function
function printString
{
    local MYSTRING="Local string!!"
    echo $MYSTRING
}
printString
```

Array
```
# Initializing array, method 1
area[11]=23
area[13]=37
area[51]=UFOs

# Initializing array, method 2
area2=( zero one two three four )

# Initializing array, method 3
area3=([17]=seventeen [24]=twenty-four)

# Dereferencing array with curly brackets {}
echo ${area3[17]}
# Note: you can have uninitialized elements in between
```

Relational operators
```
# Some relational operators, some require double parenthesis/brackets
-lt (<), -gt (>), -le (<=), -ge (>=), -eq (==), -ne (!=)
```

Arithmetic operators
```
# Some arithmetic relational operators: +, -, *, /, %
a=`expr 30 * 6`
let "a = a + 5"
let a=a+5
a=$((a - 2))
a=$(($a-=3))

# Take note on the use of space character in above variable assignments
```

if conditional block
```
if [ "$FILENAME" = "myfile.log" ]; then
    echo True
elif [ "$FILENAME" = "myfile2.log" ]; then
    echo Also True
else
    echo False
fi
```

while loop
```
while true; do
    echo Sleep for 10s
    sleep 10
done
```

for loop
```
# Sample 1 from tldp.org
for i in $( ls ); do
    echo item: $i
done

# Sample 2 from tldp.org
for i in `seq 1 10`;
do
    echo $i
done
```

case statement
```
# Note: regular expression is supported as well
case "$1" in
  start)
        echo "Starting ..."
        ;;
  stop)
        echo "Stopping ..."
        ;;
  status)
        status $httpd
        RETVAL=$?
        ;;
  restart)
        echo "Restarting ..."
        ;;
  *)
        echo "Usage: bla bla bla"
        exit 1
esac
```

Function and parameters
```
function quit {
    echo We are leaving
    exit
}

# $0 refers to the name of script file itself.
# $@ refers to all parameters received.
function printName {
    echo First Name: $2
    echo Last Name: $1
}
 
printName Maxwell Duo
quit
```

---


Sources:

1. www.tldp.org
1. www.gnu.org
