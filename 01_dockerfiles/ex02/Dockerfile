# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: ismelich <ismelich@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/01/10 15:37:31 by ismelich          #+#    #+#              #
#    Updated: 2020/01/15 09:54:55 by ismelich         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

FROM ft-rails:on-build

EXPOSE 3000
CMD [ "rails" , "s", "-b", "0.0.0.0", "-p", "3000"]

# After you build the ft-rails:on-build run this commands.
# docker build -t ex02 . && docker run -it --rm -p 3000:3000 ex02
# To test if the app is working, check your machines Char IP with the port 
# 3000 in your web browser: 192.168.99.101:3000