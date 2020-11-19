# Quick notes - compression & extraction commands

Creation date: 2008-4-25

Tags: *bash, linux, fedora, redhat*

---


**.tar** file
```
tar -cvf <output-compressed-file.tar> <space-separated-input-file-list>
tar -xvf <input-compressed-file.tar>
```

**.tar.gz** file
```
tar -czvf <output-compressed-file.tar.gz> <space-separated-input-file-list>
tar -xzvf <input-compressed-file.tar>
```

**.tar.bz2** file
```
tar -cjvf <output-compressed-file.tar.bz2> <space-separated-input-file-list>
tar -xjvf <input-compressed-file.tar.bz>
```

**.gz** file
```
gzip <output-compressed-file.gz> <space-separated-input-file-list>
gunzip <input-compressed-file.gz>
```

**.bz2** file
```
bzip2 <output-compressed-file.bz2> <space-separated-input-file-list>
bunzip2 <input-compressed-file.bz2>
```

**.zip** file
```
zip <output-compressed-file.zip> <space-separated-input-file-list>
unzip <input-compressed-file.zip>
```

**.rpm** file
```
rpm2cpio <input-compressed-file.rpm> | cpio -idmv
```
