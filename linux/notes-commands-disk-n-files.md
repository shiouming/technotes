# Quick notes - disk & files commands

Creation date: 2008-4-27

Keywords: *bash, linux, fedora, redhat*

---


Display disk space usage for each file system item relative from current directory. Level 1 means current directory only, whereby level 3 will further break down 2 more levels from current directory.

**Warning**: avoid running this command from root (/) directory.
```
du -h --max-depth=1
# show the sum of size instead of list items one by one
du -hs
```

Check the disk space usage by file system. Useful to find out if system is running out of disk space.
```
df -h
```

Splitting input file by file size. File size for each output file is up to the specified size limit. For example, let say the input_file is 22MB, following command will split it and produce 3 output files: split_myname_aa (10MB), split_myname_ab (10MB), and split_myname_ac (2MB), without altering the original input file.

**Note**: splitting a big file (i.e. 500MB) could take some time.
```
split -b 10m <input-file> <output_split_files_naming_prefix>
# e.g.
split -b 10m input_file split_myname_
```

To update the file database (/var/lib/mlocate/mlocate.db), and to search file from the same database by filename. Take note that to update the database root privilege is required.
```
updatedb
locate <filename-pattern>
```
