FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -yq --no-install-recommends \
    apt-utils \
    curl \
    # Install git
    git \
    # Install apache
    apache2 \
    # Install php
    libapache2-mod-php \
    php-cli \
    php-json \
    php-curl \
    php-fpm \
    php-gd \
    php-ldap \
    php-mbstring \
    php-mysql \
    php-soap \
    php-sqlite3 \
    php-xml \
    php-zip \
    php-intl \
    php-imagick \
    php-xdebug \
    # Install tools
    patch \
    unzip \
    openssl \
    vim \
    graphicsmagick \
    imagemagick \
    ghostscript \
    mysql-client \
    iputils-ping \
    locales \
    sqlite3 \
    ca-certificates \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set locales
RUN locale-gen en_US.UTF-8 fr_FR.UTF-8

# Configure apache
RUN a2enmod rewrite expires
RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/servername.conf
RUN a2enconf servername

# Add xhprof_support
COPY config/php/extensions/tideways_xhprof-7.2.so /usr/lib/php/20170718


EXPOSE 80 443

# Install composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && composer global require hirak/prestissimo --no-plugins --no-scripts

WORKDIR /var/www/html

RUN rm index.html

# Configure vhost
COPY config/httpd/vhost.conf /etc/apache2/sites-available/conf<project.name>.conf
RUN a2dissite 000-default
RUN a2ensite conf<project.name>.conf

# Configure PHP
COPY config/php/conf.d/*.ini /etc/php/7.2/apache2/conf.d/

# Configure XHPROF
COPY config/php/xhprof /var/www/xhprof

CMD apachectl -D FOREGROUND
