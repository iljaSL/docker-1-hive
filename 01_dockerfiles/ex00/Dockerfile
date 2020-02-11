# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: ismelich <ismelich@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/01/10 11:42:45 by ismelich          #+#    #+#              #
#    Updated: 2020/02/11 11:25:24 by ismelich         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

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