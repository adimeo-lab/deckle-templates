FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -yq  --no-install-recommends software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt-get update
    
RUN apt-get update && apt-get install -yq --no-install-recommends \
    apt-utils \
    curl \
    # Install git
    git \
    # Install apache
    apache2 \
    # Install php
    php-7.3 \
    php7.3-cli \
    php7.3-json \
    php7.3-curl \
    php7.3-fpm \
    php7.3-gd \
    php7.3-ldap \
    php7.3-mbstring \
    php7.3-mysql \
    php7.3-soap \
    php7.3-sqlite3 \
    php7.3-xml \
    php7.3-zip \
    php7.3-intl \
    php7.3-imagick \
    php7.3-xdebug \
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
COPY config/php/extensions/tideways_xhprof-7.3.so /usr/lib/php/20170718


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
COPY config/php/conf.d/*.ini /etc/php/7.3/apache2/conf.d/

# Configure XHPROF
COPY config/php/xhprof /var/www/xhprof

CMD apachectl -D FOREGROUND
