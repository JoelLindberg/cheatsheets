# Linux commands, utilities and one-liners

## Hostname

Distros: `Ubuntu`, `Fedora`

~~~
$ hostnamectl
$ hostnamectl set-hostname <name>
~~~

## Firewall - ufw

Distros: `Ubuntu`

## Firewall - firewalld

Distros: `Fedora`

~~~
sudo firewalld
~~~

Add, remove and view rules:
~~~
sudo firewall-cmd
~~~

## Search - find

Distros: All or most I would guess

Find based on a regex pattern:
~~~bash
find . -regex ".*flower.*"
~~~

*This will look for "flower" in file/dir names in the current and all subdirectories.*
