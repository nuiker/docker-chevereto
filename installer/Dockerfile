FROM php:7.2-apache
MAINTAINER "nuiker <nuiker@gmail.com>"

ENV CHEVERETO_APACHE_RUN_USER www-data

# DB connection environment variables
ENV CHEVERETO_DB_HOST db
ENV CHEVERETO_DB_USERNAME chevereto
ENV CHEVERETO_DB_PASSWORD chevereto
ENV CHEVERETO_DB_NAME chevereto
ENV CHEVERETO_DB_PREFIX chv_

# Install required packages
RUN apt-get update && apt-get install -y \
        libgd-dev

# Install php extensions that we need for Chevereto and its installer
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install \
    gd \
    mysqli \
    pdo \
    pdo_mysql \
    zip
 
# Enable mod_rewrite for Chevereto
RUN a2enmod rewrite

# Download installer script
WORKDIR /var/www/html
USER ${CHEVERETO_APACHE_RUN_USER}
RUN curl -o index.php https://chevereto.com/download/file/installer && chmod 777 index.php

COPY settings.php app/settings.php

# Expose the image directory
VOLUME /var/www/html/

# Change back to root user for normal Service start up
USER root
