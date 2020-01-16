# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: ismelich <ismelich@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/01/13 11:02:23 by ismelich          #+#    #+#              #
#    Updated: 2020/01/13 12:37:03 by ismelich         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

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