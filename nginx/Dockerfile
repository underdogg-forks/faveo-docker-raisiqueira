# PHP Base: 7.0.x
# Distro base: Linux Apline 3.5 + PHP 7.0
# Image: PHP 7.0 + Nginx Web Server
# Use for: Faveo HelpDesk IMEC

FROM ambientum/php:7.0

# Image maintainer
MAINTAINER Rai Siqueira <contato@raisiqueira.com>

USER root

# Nginx config file + Entrypoint Docker
COPY nginx.conf /etc/nginx/nginx.conf
COPY start.sh /home/ambientum/start.sh

# Install Nginx Web Server

RUN echo "--> Installing Nginx Web Server" && \
	apk add --update nginx && \
	rm -rf /tmp/* /var/tmp/* /usr/share/doc/* && \
	echo "--> Permissions" && \
	mkdir /var/run/nginx && \
	chown -R ambientum:ambientum /var/run/nginx && \
    chown -R ambientum:ambientum /var/log/nginx && \
    chown -R ambientum:ambientum /var/lib/nginx && \
    chmod +x /home/ambientum/start.sh && \
    chown -R ambientum:ambientum /home/ambientum

# Running User
USER ambientum

WORKDIR "/var/www/app"

# Get Faveo from github and composer install
RUN echo "--> Clonning Faveo repository" && \
	git clone https://github.com/ladybirdweb/faveo-servicedesk-community . && \
	echo "--> composer install" && \
	composer install && \
	composer clear-cache

# Change to root user
USER root

# Run Permissions
RUN echo "--> Change group to www-data" && \
	chgrp -R www-data . storage bootstrap/cache

# Change again to ambientum user
USER ambientum

# Expose Nginx webserver Port
EXPOSE 8080

# Start script
CMD ["/home/ambientum/start.sh"]