# Linux commands, utilities and one-liners

## vim

Preferred base .vimrc config:
~~~
set number
syntax on
colorscheme ron
set tabstop=4
" Inform vim to insert 4 spaces instead of a tab character
set expandtab
" Inform vim to always use the same distance as the tabstop when moving your text
set shiftwidth=0
~~~

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
