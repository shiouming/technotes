# User accounting from shell command line

Creation date: 2008-4-16

Keywords: useraccount, fedora

<hr>

1. To create a user account:

   `useradd <username>`


1. To assign password to an account:

   `passwd <username>`


1. To temporary disable a user account:
   1. Open the password file, either */etc/shadow* or */etc/passwd*, with text editor,
   1. Locate the entry for desired user account, and prefix the password (2nd field) with *. Simply remove the * if want to re-enable this account again.

1. To delete a user account:

   `userdel <username>`

   To delete a user account and removed all the files for the user:

   `userdel -r <username>`
