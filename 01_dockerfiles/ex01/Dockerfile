# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: ismelich <ismelich@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/01/10 12:12:19 by ismelich          #+#    #+#              #
#    Updated: 2020/01/15 10:30:14 by ismelich         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

FROM debian

MAINTAINER ismelich <ismelich@student.hive.fi>

ENV TS3SERVER_LICENSE=accept

ENV TS3SERVER_DB_HOST: IP:192.168.99.101

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