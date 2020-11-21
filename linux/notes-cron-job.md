# Quick notes - cron job basic

Creation date: 2008-6-17

Tags: *bash, linux, fedora, redhat*

---


The crontab file contains list of cron job entries, and can be edited by following command:
```
crontab -e
```

Each cron job entry will have the following columns:
```
* * * * * root echo Executed on `date`
# <minute> <hour> <DOM> <month> <DOW> <user> <command>
```

Possible values are:

**minute** : * for every minute, or specify the exact time (minute) ranges from 0 to 59.

**hour** : * for every hour, or specify the exact time (24 hours clock) ranges from 0 to 23.

**DOM** : * for every day of the month, or specify the exact DOM ranges from 1 to 31.

**month** : * for every month, or specify the exact time (24 hours clock) ranges from 1 to 12.

**DOW** : * for every day of the week, or specify the exact DOW ranges from 0 to 7 (both 0 and 7 
  refer to Sunday).

**user** : User who runs the command.

**command** : Command to be executed, space character is allowed for specifying arguments.

For **date/time values**, we can do the following
1. Use ‘-‘ dash character to specify a range. i.e. Mon-Wed for DOW, 0-6 for hour, and similar.
1. Use ‘,’ comma character to specify a list. i.e. Sep,Oct,Dec for month.
1. Use ‘/’ forward slash character to specify stepping. i.e. */4 for month refers to Apr,Aug,Dec.

Note:
1. For month and DOW, those common 3-characters values like Mon, Tue, Wed and Jan, Feb, Dec are 
  acceptable.
1. It is always recommended to specify any path in command as full path.
1. Single line comments prefixed by # character are allowed.
1. Some version of Unix such as AIX do not support 7 in DOW.
