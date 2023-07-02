# Users, Groups and Permissions

## Users and Groups

Passwords (hashed) are stored in the file `/etc/shadow`.

View users in your current system: `cat /etc/passwd` \
View groups in your current system: `cat /etc/group` \
List groups your user is a member of: `groups` \
List groups the specified user is a member of: `groups <user>` \

Add a user: `useradd <user>` \
Add a user with comment for full name: `useradd -c "<full name>" <user>` \

Delete a user: `userdel -r <user>`

*-r indicates that you implicity want the users home directory to be removed.*

The command `passwd` is not only used to change the password for a user, but also other things such as disabled/enable user, handle lockout and set expiration time.

Check the status of a user: `passwd treebeard -S`:

~~~
treebeard PS 2023-07-02 0 99999 7 -1 (Password set, SHA512 crypt.)
~~~

*PS means that the password is set.*

Source: https://www.redhat.com/sysadmin/linux-user-account-management

### Group management

Each user may belong to one primary group and any number of secondary groups. Nested groups are not allowed.

Create a new group: `groupadd <group name>`
Create a new group with a specific id: `groupadd -g 1066 <group name>`

Delete a group: `groupdel <group name>`

<br />

## Permissions

### chmod using octal notation

Reference for assigning permissions to categories: **user**, **group** and **others**:

4 = read \
2 = write \
1 = execute \
0 = no permission

Numbers are added up to represent the full permissions for each category. Using this logic the number 6 will then result in granting `write` + `read`.

For example, set **owner** to `read` + `write`, **group** to `read` and **others** to `read`:
~~~bash
chmod 0644 <target file>
~~~

The first digit 0 represents the file type, which is regular when it's 0. This can, and should be left out unless we want to modify the file type.

~~~bash
chmod 644 <target file>
~~~

<br />

### umask to set default permissions

Default permissions in the system can be set for newly created files. This can be achieved with the utility `umask`. Its value is subtracted from the base permission when a new file or directory is created. Remember a directory is a special file type. Files have a base permission of value `666` while directories have a base permission of value `777`.

`umask` 0022 results in the following if you created a test.txt file and a test directory:

~~~
drwxr-xr-x. 2 root root   6 Jul  1 15:09 test
-rw-r--r--. 1 root root   0 Jul  1 15:02 test.txt
~~~

test = 755 \
test.txt = 644

Settings set with **umask** only apply to your current session. Permanent settings can be set in your **.bashrc** file which is applied every time to you login.

<br />

## Best practices:

### Superuser

Create a new "normal" user and assign it sudo permissions using `visudo` (will edit the file `/etc/sudoers`). This way, you can be more conservative in your use of the root user account.
