# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: ismelich <ismelich@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/01/14 14:15:32 by ismelich          #+#    #+#              #
#    Updated: 2020/01/15 15:47:48 by ismelich         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

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