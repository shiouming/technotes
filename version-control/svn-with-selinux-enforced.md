# Running Subversion with SELinux enforced

Blog date: 2008-3-31

Keywords: *svn, selinux, linux, apache*

Most of the time I do development on Windows, and use TortoiseSVN for source control. Out of curiosity, this morning I tried to set up SVN server with Apache under Fedora 8, and surprisingly it did not work. Browsers threw error message — HTTP 500 under IE; error code 13 under Firefox, even though I am sure I had entered the correct user credentials.

I found the root cause from www.subversionary.org – SELinux security feature was blocking Apache from accessing SVN repo. Although we can simply disable SELinux or set it to “Permissive” mode, the more appropriate solution is by changing security context of the SVN repo directory, with following command:

```
chcon -R -h -t httpd_sys_content_t <diskpath-to-svn-repo-dir>
```
