# Users, Groups and Permissions

## Users and Groups

Passwords (hashed) are stored in the file `/etc/shadow`.

View users in your current system: `cat /etc/passwd` \
View groups in your current system: `cat /etc/group` \
List groups your user is a member of: `groups` \
List groups the specified user is a member of: `groups <user>`

Add a user: `useradd <user>` \
Add a user with comment for full name: `useradd -c "<full name>" <user>`

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

Create a new group: `groupadd <group name>` \
Create a new group with a specific id: `groupadd -g 1066 <group name>`

Change the name of a group: `groupmod -n <new group name> <old group name>` \
Change the id of a group: `groupmod -g 1033 <group name>`

Delete a group: `groupdel <group name>`

Add a user to a group: `usermod -aG <group> <user>` \
Remove a user from a group: `gpasswd -d <user> <group>`

*-aG is the same as --append and --groups.*

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

### SUID, SGID, and sticky bit

Source: https://www.redhat.com/sysadmin/suid-sgid-sticky-bit

### Superuser

*RHEL:* \
*During the installation, when you create a user and assign it "administrator" privileges, by default, it becomes a member of the group **wheel** which means it can run any command as using sudo.*

Create an admin user and assign sudo rights by making the user a member of the **wheel* group:

~~~bash
useradd <user>
passwd <user>
usermod -aG wheel <user>
~~~

Create admin user and assign using visudo or editing the /etc/sudoers file:

~~~bash
<user> ALL=(ALL) ALL
~~~

There is a configuration entry for the **wheel** group in the /etc/sudoers file. It makes allows all members of the wheel group to perform all commands using the sudo statement (providing password).

For an ordinary full system access administrator user it will be the same thing adding your user to the **wheel** group as it would be adding an entry for the user in the sudoers file.

The sudoers file however allows for more granular access control if required. To only allow `groupadd` for example, edit the sudoers file:

~~~bash
<user> ALL=/sbin/groupadd
~~~

Aliases can be created for a set of commands. In the sudoers file you could insert:

~~~bash
## Installation and management of software
Cmnd_Alias SOFTWARE = /usr/bin/dnf, /usr/bin/rpm
<group> ALL=SOFTWARE
~~~

Disable root:

To disable root, set the root shell to /sbin/nologin:

~~~bash
$ sudo sed -i 's_root:/bin/bash_root:/sbin/nologin_' /etc/passwd
~~~

Source: https://www.redhat.com/sysadmin/linux-superuser-access