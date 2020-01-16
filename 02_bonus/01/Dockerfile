# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: ismelich <ismelich@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/01/14 14:31:20 by ismelich          #+#    #+#              #
#    Updated: 2020/01/14 17:42:47 by ismelich         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

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