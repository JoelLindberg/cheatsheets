# Package Managers

## yum

Add a repository:

~~~bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
~~~

The repository configuration is stored here: `/etc/yum.repos.d/docker-ce-.repo`

