FROM php:7.0-fpm

RUN apt-get update

RUN docker-php-ext-install -j$(nproc) json mbstring opcache pdo pdo_mysql mysqli

# GMP
RUN apt-get install -y libgmp-dev
RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h
RUN docker-php-ext-install -j$(nproc) gmp

# Curl
RUN apt-get install -y libcurl4-openssl-dev
RUN docker-php-ext-install -j$(nproc) curl

# GD
RUN apt-get install -y libpng-dev libjpeg-dev
RUN docker-php-ext-install -j$(nproc) gd

# XML
RUN apt-get install -y libxml2-dev
RUN docker-php-ext-install -j$(nproc) simplexml xml soap

# Intl
RUN apt-get install -y libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

# ZIP
RUN apt-get install -y libzip-dev
RUN docker-php-ext-install -j$(nproc) zip

# APCu
RUN pecl install apcu && docker-php-ext-enable apcu

# Redis
RUN pecl install redis && docker-php-ext-enable redis

# Xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Blackfire
RUN version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") \
    && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/linux/amd64/$version \
    && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp \
    && mv /tmp/blackfire-*.so $(php -r "echo ini_get('extension_dir');")/blackfire.so \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > $PHP_INI_DIR/conf.d/blackfire.ini
