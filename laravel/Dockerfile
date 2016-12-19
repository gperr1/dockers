FROM mtmacdonald/docker-base:1.1.2
MAINTAINER Guillaume PERRIN <perrin.guillaume@gmail.com>


# run updates
RUN apt-get update && apt-get upgrade -y -o Dpkg::Options::="--force-confold"

# supervisord config files
CMD ["supervisord", "-c", "/etc/supervisord.conf"]
ADD /conf/supervisord /etc/supervisord



# nginx install and configuration
RUN apt-get -y install nginx
ADD /conf/nginx/development /etc/nginx/sites-available/default

# disable 'daemonize' in nginx (because we use supervisor instead)
RUN echo "daemon off;" >> /etc/nginx/nginx.conf


# install PHP
RUN apt-get -y install php-fpm php-cli
RUN apt-get -y install php-mbstring php-xml php-mysqlnd php-curl

# disable 'daemonize' in php-fpm (because we use supervisor instead)
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.0/fpm/php-fpm.conf

# export ports
EXPOSE 80 443

# mount volumes
VOLUME ["/share"]

# clean directories
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

