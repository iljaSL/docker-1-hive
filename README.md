You might have experienced one of these unpleasant situations:
• You’re developing on a computer and you don’t have the admin rights to install a
dependency.
• It took you five hours to install the latest version of you favorite DB but your boss
tells you the app will have to run on a previous version.
• It took you two weeks to build a perfectly clean application... that only works on
your machine.
• The SysOp in charge of all launches hates you because your app is undeployable.
Long story short, you already have or soon will spend, at some point in your life, as
much time setting up your work environment as actually developing your app.
Right now, you are probably developing a one-block app, with softwares and libraries
installed directly in your development environment, or maybe in a virtual environment...
Imagine if your application has to be deployed all over the world and you have to redevelop it for all existing platforms and OSs...
Docker was created to satisfy this need for unification and normalisation: it makes it
possible to split an application into several microservices, light, adaptable, universal and
scalable, and it also gives the system administrators a great flexibility to deploy and scale
up the app.
This suite of projects on Docker will help you better understand this specific tool, but
also the various aspects of applications development using microservices.s

///// 00_how_to_docker \\\\\

1. Create a virtual machine with docker-machine using the virtualbox driver, and
named Char:

docker-machine create --driver virtualbox Char

# ------------------------------------------------------------------------------
# Installing Homebrew on the school Mac's in order to install docker:
#
# rm -rf $HOME/.brew && git clone --depth=1 https://github.com/Homebrew/brew 
# $HOME/.brew && echo 'export PATH=$HOME/.brew/bin:$PATH' >> $HOME/.zshrc && 
# source $HOME/.zshrc && brew update
# ------------------------------------------------------------------------------
# Installing Docker:
# brew install docker docker-machine
# ------------------------------------------------------------------------------
# Move Docker to goinfre:
# mv .docker/ /goinfre/ismelich
# Create Soft Link:
# ln -s /goinfre/ismelich/.docker $HOME
# ------------------------------------------------------------------------------
# Explanation for the answer is found at the docker documentation:
# https://docs.docker.com/machine/reference/create/
# ------------------------------------------------------------------------------

2. Get the IP address of the Char virtual machine.

docker-machine ip Char

# ------------------------------------------------------------------------------
# Getting the IP address of the virtual machine:
# https://docs.docker.com/machine/reference/ip/
# ------------------------------------------------------------------------------

3. Define the variables needed by your virtual machine Char in the general env of your
terminal, so that you can run the docker ps command without errors. You have
to fix all four environment variables with one command, and you are not allowed
to use your shell’s builtin to set these variables by hand.

eval $(docker-machine env Char)

# ------------------------------------------------------------------------------
# If we run docker-machine ls we get this output:
# NAME   ACTIVE   DRIVER       STATE     URL                        ...
# Char   -        virtualbox   Running   tcp://192.168.99.100:2376  ...
# The - means that we are not connected to the virtual machine, to solve
# this error, we need to run the answer command.
# If we run the ls command again, there will be a * instead of a -
# this signals that we are now connected to our virtual machine.
# For more information: https://docs.docker.com/v17.09/machine/reference/env/
# ------------------------------------------------------------------------------

4. Get the hello-world container from the Docker Hub, where it’s available

docker pull hello-world

# ------------------------------------------------------------------------------
# For more information:
# https://docs.docker.com/engine/reference/commandline/pull/
# Checking if the pull of the image worked:
# docker image ls 
# hello-world should be listed.
# ------------------------------------------------------------------------------

5. Launch the hello-world container, and make sure that it prints its welcome message, then leaves it.

docker run hello-world

# ------------------------------------------------------------------------------
# For more information: https://docs.docker.com/engine/reference/run/
# ------------------------------------------------------------------------------

6. Launch an nginx container, available on Docker Hub, as a background task. It
should be named overlord, be able to restart on its own, and have its 80 port
attached to the 5000 port of Char. You can check that your container functions
properly by visiting
http://<ip-de-char>:5000 on your web browser.

docker run -d -p 5000:80 --name overlord --restart=always nginx

# ------------------------------------------------------------------------------
# docker run --help:
# -d, --detach — Run container in background and print container ID.
# --name string — Assign a name to the container.
# -p, --publish list — Publish a container's port(s) to the host.
# The --restart tutorial:
# https://docs.docker.com/config/containers/start-containers-automatically/
# Test if the nginx server works:
# http://192.168.99.101:5000
# ------------------------------------------------------------------------------

7. Get the internal IP address of the overlord container without starting its shell and
in one command.

docker inspect -f '{{ .NetworkSettings.IPAddress }}' overlord

# ------------------------------------------------------------------------------
# For more information about inspect:
# https://linuxconfig.org/how-to-retrieve-docker-container-s-internal-ip-address
# ------------------------------------------------------------------------------

8. Launch a shell from an alpine container, and make sure that you can interact
directly with the container via your terminal, and that the container deletes itself
once the shell’s execution is done.

docker run -it --rm alpine /bin/sh

# ------------------------------------------------------------------------------
# For more information about Docker Alpine:
# https://stackoverflow.com/questions/35689628/starting-a-shell-in-the-docker-
# alpine-container/43564198#43564198
# ------------------------------------------------------------------------------

9. From the shell of a debian container, install via the container’s package manager
everything you need to compile C source code and push it onto a git repo (of
course, make sure before that the package manager and the packages already in the
container are updated). For this exercise, you should only specify the commands
to be run directly in the container.

apt-get update && apt-get upgrade -y && apt-get install -y build-essential git

# ------------------------------------------------------------------------------
# First run this command to run debian:
# docker run -ti --rm debian
# For more information about compilers:
# https://www.cyberciti.biz/faq/debian-linux-install-gnu-gcc-compiler/
# For the latest stable version for your release of Debian/Ubuntu:
# apt-get install git
# ------------------------------------------------------------------------------

10. Create a volume named hatchery.

docker volume create --name hatchery

# ------------------------------------------------------------------------------
# More informations abot creating docker volume:
# https://docs.docker.com/engine/reference/commandline/volume_create/
# ------------------------------------------------------------------------------

11. List all the Docker volumes created on the machine. Remember. VOLUMES.

docker volume ls

# ------------------------------------------------------------------------------
# More information about docker volume ls:
# https://docs.docker.com/engine/reference/commandline/volume_ls/
# ------------------------------------------------------------------------------

12. Launch a mysql container as a background task. It should be able to restart on its
own in case of error, and the root password of the database should be Kerrigan.
You will also make sure that the database is stored in the hatchery volume, that
the container directly creates a database named zerglings, and that the container
itself is named spawning-pool.

docker run -d --name spawning-pool --restart=on-failure:10 -e MYSQL_ROOT_PASSWORD=Kerrigan -e MYSQL_DATABASE=zerglings -v hatchery:/var/lib/mysql mysql --default-authentication-plugin=mysql_native_password

# ------------------------------------------------------------------------------
# docker -d = --detach -> container starts up and run in background.
# --restart=on-failure:10 -> Restart the container if it stops.
# docker -e -> Set environment variables.
# MYSQL: Open-Source database management system
# For more informations: https://hub.docker.com/_/mysql
# MYSQL_ROOT_PASSWORD -> Its mandatory to set a password.
# MYSQL_DATABASE -> Allows you to specify the name of a database in our case
# zerglings.
# mysql --default-authentication-plugin=mysql_native_password ->
# is needed for wordpress, its not working otherwise with mysql latest
# container, mysql changed the password authenticication method.
# For more information:
# https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html
# ------------------------------------------------------------------------------

13. Print the environment variables of the spawning-pool container in one command,
to be sure that you have configured your container properly.

docker inspect -f '{{.Config.Env}}' spawning-pool

# ------------------------------------------------------------------------------
# For more information about docker inspect:
# https://docs.docker.com/engine/reference/commandline/inspect/
# ------------------------------------------------------------------------------

14. Launch a wordpress container as a background task, just for fun. The container
should be named lair, its 80 port should be bound to the 8080 port of the virtual
machine, and it should be able to use the spawning-pool container as a database
service. You can try to access lair on your machine via a web browser, with the
IP address of the virtual machine as a URL.
Congratulations, you just deployed a functional Wordpress website in two commands!

docker run -d --name lair -p 8080:80 --link spawning-pool -e WORDPRESS_DB_HOST='spawning-pool' -e WORDPRESS_DB_USER='root' -e WORDPRESS_DB_PASSWORD='Kerrigan' -e WORDPRESS_DB_NAME='zerglings' wordpress

# ------------------------------------------------------------------------------
# More informations aboout Legacy container links:
# https://docs.docker.com/network/links/
# Check if the deployment worked in your web browser:
# http://192.168.99.101:8080/
# ------------------------------------------------------------------------------

15. Launch a phpmyadmin container as a background task. It should be named roach-warden,
its 80 port should be bound to the 8081 port of the virtual machine and it should
be able to explore the database stored in the spawning-pool container.

docker run --name roach-warden  -d --link spawning-pool:db -p 8081:80 phpmyadmin/phpmyadmin

# ------------------------------------------------------------------------------
# Phpmyadmin must point to MySQL Server. So that we must link both containers 
# by adding the option : –link name-of-container:name-of-imag.
# More about how to connect PhpMyAdmin container to MySQL:
# https://omarghader.github.io/docker-tutorial-phpmyadmin-and-mysql-server/
# To check if it worked check http://192.168.99.101:8081/ in your browser.
# Login: root PW: Kerrigan
# ------------------------------------------------------------------------------

16. Look up the spawning-pool container’s logs in real time without running its shell.

docker logs -f spawning-pool

# ------------------------------------------------------------------------------
# How to view a docker container log in real time:
# https://success.docker.com/article/view-realtime-container-logging
# ------------------------------------------------------------------------------

17. Display all the currently active containers on the Char virtual machine.

docker ps -a

# ------------------------------------------------------------------------------
# docker ps --help:
# -a -> all
# ps -> list all containers
# ------------------------------------------------------------------------------

18. Relaunch the overlord container.

docker restart overlord

# ------------------------------------------------------------------------------
#  docker restart: https://docs.docker.com/engine/reference/commandline/restart/
#  With the docker ps -a you can check in Status if the restart has worked.
# ------------------------------------------------------------------------------

19. Launch a container name Abathur. It will be a Python container, 2-slim version,
its /root folder will be bound to a HOME folder on your host, and its 3000 port
will be bound to the 3000 port of your virtual machine.
You will personalize this container so that you can use the Flask micro-framework
in its latest version. You will make sure that an html page displaying "Hello World"
with <h1> tags can be served by Flask. You will test that your container is
properly set up by accessing, via curl or a web browser, the IP address of your
virtual machine on the 3000 port.
You will also list all the necessary commands in your repository.

docker run --name Abathur -v ~/:/root -p 3000:3000 -dit python:2-slim
docker exec Abathur pip install Flask
echo 'from flask import Flask\napp = Flask(__name__)\n@app.route("/")\ndef hello_world():\n\treturn "<h1>Hello, World!</h1>"' > ~/app.py
docker exec -e FLASK_APP=/root/app.py Abathur flask run --host=0.0.0.0 --port 3000

# ------------------------------------------------------------------------------
# The commands can NOT be runned all at the same time otherwise you will get a 
# syntax error, launch them one at a time.
# docker exec -> Run a command in a running container, more information:
# https://docs.docker.com/engine/reference/commandline/exec/
# -v -> volume list
# -p -> pubish list
# pip -> install and update Flask 
# Flask(web framework) - https://flask.palletsprojects.com/en/1.0.x/quickstart/
# To run the application you can either use the flask command or python’s -m 
# switch with Flask. Before you can do that you need to tell your terminal the 
# application to work with by exporting the FLASK_APP environment variable:
# $ export FLASK_APP=hello.py
# $ flask run
#  * Running on http://127.0.0.1:5000/
# Check if it worked on your web browser:
# http://192.168.99.101:3000/
# or curl $(docker-machine ip Char):3000
# curl -> a tool to transfer data from or to a server.
# ------------------------------------------------------------------------------

20. Create a local swarm, the Char virtual machine should be its manager.

eval $(docker-machine env Char)
docker swarm init --advertise-addr $(docker-machine ip Char)
docker swarm join-token worker -q > ~/.worker_token

# ------------------------------------------------------------------------------
# How to create a swarm:
# https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/
# How to check the nodes to see if my machine is the manager:
# docker node ls
# docker info
# For more information:
# https://docs.docker.com/engine/swarm/admin_guide/
# ------------------------------------------------------------------------------

21. Create another virtual machine with docker-machine using the virtualbox driver,
and name it Aiur.

docker-machine create --driver virtualbox Aiur

# ------------------------------------------------------------------------------
# How to check if the creation worked: docker-machine ls
# ------------------------------------------------------------------------------

22. Turn Aiur into a slave of the local swarm in which Char is leader (the command to
take control of Aiur is not requested).

eval $(docker-machine env Aiur)
docker swarm join --token $(cat ~/.worker_token) $(docker-machine ip Char):2377

# ------------------------------------------------------------------------------
# Activate the vm Char again: eval $(docker-machine env Char)
# Check the changes with: docker node ls and docker info 
# ------------------------------------------------------------------------------

23. Create an overlay-type internal network that you will name overmind.

docker network create -d overlay overmind

# ------------------------------------------------------------------------------
# -d (--driver string) -> Driver to manage the Network, by default its "bridge".
# For more information about docker network:
# https://docs.docker.com/network/overlay/
# ------------------------------------------------------------------------------

24. Launch a rabbitmq SERVICE that will be named orbital-command. You should
define a specific user and password for the RabbitMQ service, they can be whatever
you want. This service will be on the overmind network.

docker service create -d --network overmind --name orbital-command -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=root rabbitmq

# ------------------------------------------------------------------------------
# -d -> Exit immediately instead of waiting for the service to converge.
# -e -> Set environment variables.
# For more information about the RabbitMQ (open source multi-protocol messaging 
# broker) Image in Docker:
# https://hub.docker.com/_/rabbitmq/
# ------------------------------------------------------------------------------

25. List all the services of the local swarm.

docker service ls

# ------------------------------------------------------------------------------
# ls -> list services
# More information about docker services:
# https://docs.docker.com/engine/reference/commandline/service/
# ------------------------------------------------------------------------------

26. Launch a 42school/engineering-bay service in two replicas and make sure that
the service works properly (see the documentation provided at hub.docker.com).
This service will be named engineering-bay and will be on the overmind network.

docker service create -d --network overmind --name engineering-bay --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/engineering-bay

# ------------------------------------------------------------------------------
# For more information about engineering-bay:
# https://hub.docker.com/r/42school/engineering-bay/
# How to use:
# You must have an orbital-command running on your host or swarm, accessible 
# with the same name into your network.
# To connect to this orbital-command, you must set a
# OC_USERNAME : Username used to access to orbital-command
# OC_PASSWD : Password used to access to orbital-command
# Check if it worked:
# docker service ls
# ------------------------------------------------------------------------------

27. Get the real-time logs of one the tasks of the engineering-bay service

docker service logs engineering-bay

# ------------------------------------------------------------------------------
# For more informations about docker service logs:
# https://docs.docker.com/engine/reference/commandline/service_logs/
# ------------------------------------------------------------------------------

28. ... Damn it, a group of zergs is attacking orbital-command, and shutting down
the engineering-bay service won’t help at all... You must send a troup of Marines
to eliminate the intruders. Launch a 42school/marine-squad in two replicas,
and make sure that the service works properly (see the documentation provided
at hub.docker.com). This service will be named... marines and will be on the
overmind network.

docker service create -d --network overmind --name marines --replicas 2 -e OC_USERNAME=root -e OC_PASSWD=root 42school/marine-squad

# ------------------------------------------------------------------------------
# For more information about engineering-bay:
# https://hub.docker.com/r/42school/marine-squad
# Check if it worked:
# docker service ls
# ------------------------------------------------------------------------------

29. Display all the tasks of the marines service.

docker service ps marines

# ------------------------------------------------------------------------------
# docker service ps -> list the tasks of one or more services.
# For more informations:
# https://docs.docker.com/engine/reference/commandline/service_ps/
# ------------------------------------------------------------------------------

30. Increase the number of copies of the marines service up to twenty, because there’s
never enough Marines to eliminate Zergs. (Remember to take a look at the tasks
and logs of the service, you’ll see, it’s fun.)

docker service scale -d marines=20

# ------------------------------------------------------------------------------
#  For more informations about docker service scale:
# https://docs.docker.com/engine/reference/commandline/service_scale/
# Check the logs, its fun!
# docker service logs -f $(docker service ps marines -f "name=marines.11" -q)
# ------------------------------------------------------------------------------

31. Force quit and delete all the services on the local swarm, in one command.

docker service rm $(docker service ls -q)

# ------------------------------------------------------------------------------
# rm -> remove one or more containers.
# -q -> quit
# Check for result:
# docker service ls
# ------------------------------------------------------------------------------

32. Force quit and delete all the containers (whatever their status), in one command.

docker rm -f $(docker ps -a -q)

# ------------------------------------------------------------------------------
# rm -> remove one or more containers
# -f -> force the romoval of a running containers
# Check for result:
# docker ps -a
# ------------------------------------------------------------------------------

33. Delete all the container images stored on the Char virtual machine, in one command
as well.

docker rmi $(docker images -a -q)

# ------------------------------------------------------------------------------
# rmi -> remove one or more images 
# -a -> all
# -q -> quite
# Check the Result:
# docker images ls
# ------------------------------------------------------------------------------

34. Delete the Aiur virtual machine without using rm -rf

docker-machine rm -y Aiur

# ------------------------------------------------------------------------------
# Checkt the result:
# docker-machine ls
# ------------------------------------------------------------------------------

///// 01_dockerfiles \\\\\

Exercise 00: vim/emacs
From an alpine image you’ll add to your container your favorite text editor, vim or
emacs, that will launch along with your container.

FROM alpine

MAINTAINER ismelich <ismelich@student.hive.fi> 

# Set alpine to the newest version and install vim while lunching alpine
RUN apk update && apk upgrade && apk add vim

# Launch vim when we enter alpine
ENTRYPOINT vim

# docker build -t ex00 . | -t -> tag list 
# docker run -it --rm ex00 
# --rm -> Automatically remove the container when it exits
# More information about docker build and run:
# https://docs.docker.com/engine/reference/run/
# https://docs.docker.com/engine/reference/commandline/build/
# Awesome Cheat Sheet for Dockerfiles:
# https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index
# Just for fun to see if MAINTAINER worked:
# docker inspect ex00 | grep "Author"
# To see if the creation of the image worked: docker image ls

Exercise 01: BYOTSS
From a debian image you will add the appropriate sources to create a TeamSpeak
server, that will launch along with your container. It will be deemed valid if at least
one user can connect to it and engage in a normal discussion (no far-fetched setup), so
be sure to create your Dockerfile with the right options. Your program should get the
sources when it builds, they cannot be in your repository.
For the smarty-pants using the official docker image of TeamSpeak is
strictly FORBIDDEN, and will get you -42.

FROM debian

MAINTAINER ismelich <ismelich@student.hive.fi>

ENV TS3SERVER_LICENSE=accept

WORKDIR /home/teamspeak

EXPOSE 9987/udp 10011 30033

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y wget bzip2 && \
    wget http://dl.4players.de/ts/releases/3.8.0/teamspeak3-server_linux_amd64-3.8.0.tar.bz2 && \
    tar -xvf teamspeak3-server_linux_amd64-3.8.0.tar.bz2

WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT sh ts3server_minimal_runscript.sh

# ENV TS3SERVER_LICENSE=accept -> It's necessary to override the env-variable
# TS3SERVER with accept otherwise the server will just print an info about the 
# licence not being accepted and WILL NOT START.
# WORKDIR -> Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, 
# and ADD instructions that follow it.

# EXPOSE -> Informs Docker that the container listens on the specified network 
# port(s) at runtime. The nubers are the ports to the teamspeak host.

# RUN -> we run debian install and upgrade the latest version, we get bzip2 
# open source data compressor to open the teamspeak server and get the ts server
# from 4 players. Check for newest version on this website:
# http://dl.4players.de/ts/releases/

# docker build -t ex01 . && docker run -it --rm -p=9987:9987/udp  -p=10011:10011 -p=30033:30033 ex01

# Launch the TS3 Client, connect to your server: Ther server name is my machine IP
# and the password is the token number we will receive while running the Dockerfile.
# SET UP VM SO A USER CAN JOIN YOUR TS3 SERVER:
# Stop the VM Char: docker-machine stop Char
# Go to the VM settings/network/adapter3/enable network adapter/bridge adappter/en0:Ethernet/
# advanced/adapter type /intel pro/1000 MT Desktop/ ok
# Start the machine again: docker-machine start Char and activeted it!
# run this command: docker-machine regenerate-certs Char 
# connect via ssh to your VM: docker-machine ssh Char 
# ifconfig -a / copy the inet add: 10.12.180.206 from eth2.
# exit ssh connection 
# Build and Run the Ts3 server
# Connect in the Client with the eth2 IP and the Token you received
# A User can join now your server.

Exercise 02: Dockerfile in a Dockerfile... in a
Dockerfile ?
You are going to create your first Dockerfile to containerize Rails applications. That’s
a special configuration: this particular Dockerfile will be generic, and called in another
Dockerfile, that will look something like this:
FROM ft-rails:on-build
EXPOSE 3000
CMD ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
Your generic container should install, via a ruby container, all the necessary dependencies and gems, then copy your rails application in the /opt/app folder of your
container. Docker has to install the approtiate gems when it builds, but also launch
the migrations and the db population for your application. The child Dockerfile should
launch the rails server (see example below). If you don’t know what commands to use,
it’s high time to look at the Ruby on Rails documentation.

FROM ft-rails:on-build

EXPOSE 3000
CMD [ "rails" , "s", "-b", "0.0.0.0", "-p", "3000"]

# After you build the ft-rails:on-build run this commands.
# docker build -t ex02 . && docker run -it --rm -p 3000:3000 ex02
# To test if the app is working, check your machines Char IP with the port 
# 3000 in your web browser: 192.168.99.101:3000

FROM ruby:2.5.1

MAINTAINER ismelich <ismelich@student.hive.fi>

RUN apt-get update && apt-get install -y nodejs
RUN gem install rails --version 5.2.0

RUN mkdir -p /opt/app
COPY ./app /opt/app

ONBUILD WORKDIR /opt/app
ONBUILD RUN bundle install
ONBUILD RUN rake db:create
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# git clone https://github.com/RailsApps/rails-signup-thankyou.git app
# docker build -t ft-rails:on-build .

# The rails app should be copied to the /opt/app folder. 
# ONBUILD -> Adds to the image a trigger instruction to be executed at a later time, 
# when the image is used as the base for another build. 
# ONBUILD RUN rake -> Create your database, migrate and/or seed(sharing) it.
# Deploy docker containers via Rake. This is especially useful for dockerized 
# Ruby applications.

Exercise 03: ... and bacon strips ... and bacon
strips ...
Docker can be useful to test an application that’s still being developed without polluting your libraries. You will have to design a Dockerfile that gets the development
version of Gitlab - Community Edition installs it with all the dependencies and the necessary configurations, and launches the application, all as it builds. The container will be
deemed valid if you can access the web client, create users and interact via GIT with this
container (HTTPS and SSH). Obviously, you are not allowed to use the official container
from Gitlab, it would be a shame...

FROM ubuntu:16.04

# Suppported installations:
# https://about.gitlab.com/install/

MAINTAINER ismelich <ismelich@studetend.hive.fi>

ENV TERM=xterm

# Without setting a terminal variable to the shell of your choice we get the 
# following error message: Environment variable: TERM has not been set

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y xterm ca-certificates openssh-server wget postfix

# Now we need to install and coniguure the necessary dependencies, with postfix
# we can send notification emails.
# https://about.gitlab.com/install/#ubuntu

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && chmod 777 script.deb.sh && ./script.deb.sh && apt-get install -y gitlab-ce

RUN apt update && apt install -y tzdata && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN sed -i "s/# grafana\['enable'\] = true/grafana['enable'] = false/g" /etc/gitlab/gitlab.rb

# sed->stream editor for filtering and transforming text
# -i -> edit files in place
# Error “ruby_block[authorize Grafana with GitLab] action run”
# Grafana is is used to monitor things happening on GitLab, but it can 
# take an eternity to authorize, and sometimes it down right refuses to initialize properly.
# https://docs.gitlab.com/omnibus/settings/grafana.html

EXPOSE 443 80 22

# We need to expose the needed ports for GitLab
# https://docs.gitlab.com/omnibus/package-information/defaults.html

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

# We set our entry point to reconfigure our GitLab setup, which will also
# start GitLab after the setup is completed.
# The tail is needed to keep Docker Containers Runnig:
# http://bigdatums.net/2017/11/07/how-to-keep-docker-containers-running/

# COMMAND FOR BUILDING AND RUNNING THE DOCKERFILE:
# docker build -t ex03 . && docker run -it --shm-size=4g --rm -p 8080:80 -p 8022:22 -p 8443:443 --privileged ex03
# We can change the virtual memory size with the --shm-size parameter, when we run the container,
# to speed up the Slow Startup Time:
# https://www.cyberciti.biz/tips/what-is-devshm-and-its-practical-usage.html
# --privileged -> We need rights to write to the docker container file system, with
# privileged we get that result.

# Check the result in your web browser: 
# http://192.168.99.101:8080
# We need to create first a new password, after that we can log in wiht root and
# use GitLabCE.

///// 02_bonus  \\\\\

Now that you understand all the subtleties of Docker, you can enjoy yourself for the
bonuses!
00 - C ENV

FROM debian

MAINTAINER ismelich <ismelich@student.hive.fi>

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y \
	curl \
	vim \
	gcc \
	git \
	zsh

ADD https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh .

USER root

RUN chmod 777 install.sh && ./install.sh && rm install.sh \
	&& cd ~ && echo "alias gccc='gcc -Wall -Wextra -Werror \$*'" >> .zshrc \
	&& mkdir /projects

CMD cd /projects && zsh

# docker build -t 01 . && docker run --rm -ti 01

01 - Minecraft Server

FROM debian:stretch

# Stretch, so not all versions of Debian images are downloaded.

MAINTAINER ismelich <ismelich@student.hive.fi>

ENV MINECRAFT_UTILITY https://github.com/marblenix/minecraft_downloader/releases/download/latest/minecraft_downloader_linux

# Version of minecraft to download

ENV MINECRAFT_VERSION latest

RUN apt update; \
    apt install -y default-jre ca-certificates-java curl; \
    curl -sL "${MINECRAFT_UTILITY}" -o minecraft_downloader; \
    chmod +x ./minecraft_downloader; \
    ./minecraft_downloader -o minecraft_server_${MINECRAFT_VERSION}.jar;

WORKDIR /data
VOLUME /data

# Sets working directory for the CMD instruction (also works for RUN, ENTRYPOINT commands)
# Create mount point, and mark it as holding externally mounted volume

EXPOSE 25565

# Expose the container's network port: 25565 during runtime.


CMD echo eula=true > /data/eula.txt && java -jar /minecraft_server_${MINECRAFT_VERSION}.jar
# Automatically accept Minecraft EULA, and start Minecraft server

# docker build -t 02 . && docker run --rm -d -p 25565:25565 02
