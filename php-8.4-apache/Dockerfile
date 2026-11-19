FROM php:8.4.28-apache@sha256:decf0c9223a74166bc7f4e29a8c725f55d94e4837b5ead7b4cd178efda871943

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

#PHP INSTALLATION
RUN apt-get update && apt-get install -y \
    git \
    gnupg \
    libfreetype-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libldap2-dev \
    libmagickwand-dev \
    libpng-dev \
    libxml2-dev \
    libzip-dev \
    tmux \
    vim \
    xz-utils \
    zip

RUN docker-php-ext-configure gd --with-freetype --with-jpeg

RUN docker-php-ext-install \
    gd \
    ldap \
    mysqli \
    opcache \
    pcntl \
    pdo_mysql \
    soap \
    zip \
    intl \
    exif \
    ftp

# Imagick and APCu; xDebug installed but disabled, as before
RUN pecl install imagick apcu xdebug && docker-php-ext-enable imagick apcu

RUN echo "memory_limit=-1" > /usr/local/etc/php/conf.d/memory-limit-php.ini

# Node
RUN curl -sSL https://nodejs.org/dist/v11.15.0/node-v11.15.0-linux-x64.tar.xz | \
    tar -xJ -C /usr/local --strip-components=1 --exclude='*.md' --exclude=LICENSE

# Install yarn and gulp
RUN npm install -g yarn gulp-cli

# Install Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

WORKDIR /code

#Composer installation (Composer 1 doesn't support PHP 8.4+)
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer

#APACHE INTALLATION
RUN a2enmod rewrite headers
