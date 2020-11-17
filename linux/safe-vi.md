# Mum say: don’t crash your server box with vi …

Blog date: 2008-4-21

Keywords: *vim, vi, linux, fedora, redhat, bash*

---

Have you ever make your server crash or hang before, by opening large log file? It happened to me. When I just joined the company, once I was assigned to troubleshoot a production issue. The application was running on an old server that having rather small memory, and I wasn't aware that log rotator was having bug, it won't rotate log file in certain situation. Without checking the file size, I went ahead to open the application log with vi ... Boom!

Below Bash script is my quick workaround to prevent the same issue from happening again, before the log rotator get fixed.

```
#!/bin/bash
# DATE    : 2008-04-23
# VERSION : 0.2
# USAGE   : safevi <filename> [vi]
# NOTE    : Use this script at your own risk


# Set the warning and blocking level here to fit your needs
let "WARNING_LEVEL_MB=10"
let "BLOCKING_LEVEL_MB=50"
let "WARNING_LEVEL=$WARNING_LEVEL_MB*1024*1024"
let "BLOCKING_LEVEL=$BLOCKING_LEVEL_MB*1024*1024"

# Decide the editor to be use, either vim (default) or vi
EDITOR="vim"
if [ "$2" = "vi" ]; then
        echo "safevi: use $2 instead of $EDITOR"
        EDITOR="vi"
fi

# Check if filename is supplied in parameters
FILENAME=$1
if [ "$FILENAME" = "" ]; then
        echo "safevi: parameter contains no filename ==> open new buffer"
        $EDITOR
        exit 0
fi

# Check if the file exists
if [ ! -e "$FILENAME" ]; then
        echo "safevi: $FILENAME does not exist ==> open $FILENAME as new buffer"
        $EDITOR $FILENAME
        exit 0
fi

# Check the file size
FILESIZE=$(stat -c%s "$FILENAME")
echo "File size of $FILENAME is $FILESIZE bytes"

# Choose the appropriate action base on filesize detected
if [ "$FILESIZE" -gt "$BLOCKING_LEVEL" ]; then
        echo "safevi: file size over the blocking level of $BLOCKING_LEVEL_MB MB ==> Exit!"
elif [ "$FILESIZE" -gt "$WARNING_LEVEL" ]; then
        echo "safevi: file size over the warning level of $WARNING_LEVEL_MB MB"
        echo "Are you sure want to open this file? [y/n]"
        read -e USER_OPTION
        if [ "$USER_OPTION" = "y" ]; then
                echo "safevi: opening $FILENAME with $EDITOR"
                $EDITOR $FILENAME
        else
                echo "safevi: exit!"
        fi
else
        echo "safevi: opening $FILENAME with $EDITOR"
        $EDITOR $FILENAME
fi

exit 0
```

Save this script as **safevi.sh** and do the following:

```
chmod 775 safevi.sh
alias vi="<fullpath_to_safevi_script>"
```

Voila!! Now when you use vi command to open a text file, the script will do a checking on file size beforehand.

To apply the alias automatically in future login session, set it in shell startup profile:
```
<user_home_dir>/.bashrc
```

— Edited on 23 April 2008 —

The script file had been updated to version 0.2 to handle following scenarios:
   * parameters contain no filename
   * the filename is a non-existing file
