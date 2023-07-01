# Linux commands, utilities and one-liners

## cat, tail and head

Multi-line output to a file using `cat`:

~~~bash
cat <<-END > index.html
<html>
    <body>
        <h1>Hello!</h1>
    </body>
</html>
END
~~~

## vim

My preferred base .vimrc config below. I use 2 tabstops to be able to work with both yaml and python. Another reason for 2 tabstops is also that [Google's style guide specifies 2 tabstops](https://google.github.io/styleguide/shellguide.html#s5.1-indentation) as preferred for bash scripts. While that shouldn't matter for me personally it might be a useful beacon since other coders also might adapt to that standard.

~~~
set number
syntax on
colorscheme ron
set tabstop=2
set autoindent
" Inform vim to insert 4 spaces instead of a tab character
set expandtab
" Inform vim to always use the same distance as the tabstop when moving your text
set shiftwidth=0
~~~

Check which vim config file that is being used. While in vim you can enter the command: `echo $MYVIMRC`

<br />

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
