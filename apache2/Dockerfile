
FROM php:7.4-apache
LABEL maintainer="ruman.eee@gmail.com"

# Download script to install PHP extensions and dependencies
#ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod uga+x /usr/local/bin/install-php-extensions && sync

RUN DEBIAN_FRONTEND=noninteractive apt-get update -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -qq -y \
      curl \
      git \
      zip unzip \
    && install-php-extensions \
      bcmath \
      bz2 \
      calendar \
      exif \
      gd \
      intl \
      ldap \
      memcached \
      mysqli \
      opcache \
      pdo_mysql \
      pdo_pgsql \
      pgsql \
      redis \
      soap \
      xsl \
      zip \
      sockets \
    && a2enmod rewrite

RUN apt update && apt install openssl -yqq
#RUN apt install mod_ssl && a2enmod ssl

## Enable SSL
RUN a2enmod ssl

RUN openssl genrsa -out "/etc/ssl/certs/myopenssl.local.key" 2048 \
  && openssl req -new -key "/etc/ssl/certs/myopenssl.local.key" -out "/etc/ssl/certs/myopenssl.local.csr" -subj "/CN=myopenssl.local/O=myopenssl.local/C=UK" \
  && openssl x509 -req -days 365 -in "/etc/ssl/certs/myopenssl.local.csr" -signkey "/etc/ssl/certs/myopenssl.local.key" -out "/etc/ssl/certs/myopenssl.local.crt"

EXPOSE 80 443

COPY vhost.conf /etc/apache2/sites-enabled/vhost.conf

# Enable Mod Rewrite
#RUN a2enmod rewrite

# User mod the users so they run under the correct permissions.
RUN usermod -u 1000 www-data
RUN usermod -G staff www-data