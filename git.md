# Git

## Mandatory config

git config --global user.name "your name"\
git config --global user.email "your email"


## Change remote

In case you accidentally cloned a repo using https, made a lot of changes and want to push using ssh. Quickest way is to change the remote.

Verify what is currently set:
~~~bash
git remote -v
~~~

Change the remote:
~~~bash
git remote set-url origin <new remote link>
~~~