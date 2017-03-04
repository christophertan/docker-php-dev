FROM php:7-apache

# Set default ServerName
RUN echo "ServerName localhost" > /etc/apache2/conf-enabled/servername.conf

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
  --install-dir=/usr/local/bin \
  --filename=composer

# Install packages
RUN apt-get update && apt-get -y install \
      sqlite3

# Enable mod_rewrite
RUN a2enmod rewrite

# Copy php configuration
COPY config/php.ini /usr/local/etc/php/

# Copy application code
COPY src /var/www/html

# Add www user for development purposes
RUN useradd -u 1000 -c www -s /usr/sbin/nologin -M -d /var/www www > /dev/null 2>&1
ENV APACHE_RUN_USER www
ENV APACHE_RUN_GROUP www