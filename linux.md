# Linux commands, utilities and one-liners

## SSH

### SSH config file

Github:
~~~
Host github.com
    User git
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/<private key file name>
    IdentitiesOnly yes
~~~

Practical way to store different ssh hosts to be used as aliases when connecting:
~~~
Host dev
    HostName dev.example.com
    User <username>
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/<private key file name>
~~~

To connect to the above: `ssh dev`

<br />
<br />

## Hostname

Distros: `Ubuntu`, `Fedora`

~~~
$ hostnamectl
$ hostnamectl set-hostname <name>
~~~

<br />
<br />

## Firewall - ufw

Distros: `Ubuntu`

<br />
<br />

## Firewall - firewalld

Distros: `Fedora`

~~~
sudo firewalld
~~~

Add, remove and view rules:
~~~
sudo firewall-cmd
~~~

<br />
<br />

## Search - find

Distros: All or most I would guess

Find based on a regex pattern:
~~~bash
find . -regex ".*flower.*"
~~~

*This will look for "flower" in file/dir names in the current and all subdirectories.*
