FROM php:7-apache

# Set default ServerName
RUN echo "ServerName localhost" > /etc/apache2/conf-enabled/servername.conf

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
  --install-dir=/usr/local/bin \
  --filename=composer

# Nullmailer debconf selections
RUN echo "nullmailer shared/mailname string localhost" | debconf-set-selections
RUN echo "nullmailer nullmailer/relayhost string localhost smtp --port=1025" | debconf-set-selections

# Install packages
RUN DEBIAN_FRONTEND=non-interactive apt-get update && apt-get -y install \
      sqlite3 \
      zlib1g-dev \
      libpng-dev \
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libxml2-dev \
      nullmailer && \
      docker-php-ext-install mysqli && \
      docker-php-ext-install mbstring && \
      docker-php-ext-install soap && \
      docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/  && \
      docker-php-ext-install gd

# Enable mod_rewrite
RUN a2enmod rewrite

# Add www user for development purposes
RUN useradd -u 1000 -c www -s /usr/sbin/nologin -M -d /var/www www > /dev/null 2>&1

ENV APACHE_RUN_USER www
ENV APACHE_RUN_GROUP www
ENV APACHE_DOC_ROOT ""

ENV NULLMAILER_HOST localhost
ENV NULLMAILER_PORT 1025

# Copy application code
COPY src /var/www/html

# Copy configuration
COPY config/php.ini /usr/local/etc/php
COPY config/docker-php-entrypoint /usr/local/bin
COPY config/000-default.conf /etc/apache2/sites-available
