# Docker Apache with PHP development environment

Host your php development environment using docker while developing on locally mounted files.

A program inside a docker container must run in the foreground. This is why the apache2 service is not starting automatically and we start it manually in the foreground with a CMD statement in the Dockerfile.

The below Dockerfile should be created from within your folder with your php app. It assumes your php files are put in a subfolder named *app*. It will mount the *app* folder to the published apache folder */var/www/html*.

Folder structure:

/myproject/Dockerfile \
/myproject/app/index.php

1. Create a Dockerfile:

~~~dockerfile
FROM ubuntu:22.04
#COPY ./app/ /var/www/html/

RUN apt-get update

# Set the timezone: Required when installing one of the php packages
RUN apt-get install tzdata
RUN echo 'Europe/Stockholm' > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# Install apache and php
RUN apt-get install -y apache2
RUN apt-get install -y php-common libapache2-mod-php php-cli
# service auto start
RUN update-rc.d apache2 enable
RUN /etc/init.d/apache2 start

# Configure apache
#RUN sed -i '/ServerName/ s/ServerName 0.0.0.0//g' /etc/apache2/apache2.conf
#RUN echo "ServerName 0.0.0.0" >> /etc/apache2/apache2.conf

# Start apache
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
~~~

2. Build an image:

~~~bash
docker build -t apache-php-dev .
~~~

3. Start a container based on the built image:

~~~bash
docker run -d -p 8080:80 \
--mount type=bind,source="$(pwd)"/app,target=/var/www/html \
-it apache-php-dev
~~~

*The source foulder should be the folder where you keep your app's source files.*

4. Visit the php app: `http://127.0.0.1:8080`

Start developing!