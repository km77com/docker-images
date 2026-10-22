FROM debian:trixie-slim

ARG PHP=7.4

# PHP from deb.sury.org, which keeps backporting security fixes to EOL PHP versions
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates curl && \
    curl -sSLo /tmp/debsuryorg-archive-keyring.deb https://packages.sury.org/debsuryorg-archive-keyring.deb && \
    dpkg -i /tmp/debsuryorg-archive-keyring.deb && rm /tmp/debsuryorg-archive-keyring.deb && \
    echo "deb [signed-by=/usr/share/keyrings/debsuryorg-archive-keyring.gpg] https://packages.sury.org/php/ trixie main" > /etc/apt/sources.list.d/php.list

#PHP INSTALLATION
RUN apt-get update && apt-get install -y \
    apache2 \
    libapache2-mod-php$PHP \
    php$PHP-apcu \
    php$PHP-cli \
    php$PHP-curl \
    php$PHP-gd \
    php$PHP-imagick \
    php$PHP-intl \
    php$PHP-json \
    php$PHP-ldap \
    php$PHP-mbstring \
    php$PHP-mysql \
    php$PHP-opcache \
    php$PHP-readline \
    php$PHP-soap \
    php$PHP-sqlite3 \
    php$PHP-xdebug \
    php$PHP-xml \
    php$PHP-zip \
    git \
    gnupg \
    tmux \
    vim \
    xz-utils \
    zip

# xDebug installed but disabled, as before
RUN phpdismod xdebug

# Keep the settings of the previous php:*-apache based image: Debian's php.ini disables
# session GC (it relies on a cron job that doesn't run in containers) and pcntl_* functions
RUN printf 'memory_limit=-1\nsession.gc_probability=1\ndisable_functions=\n' > /etc/php/$PHP/mods-available/km77.ini && \
    phpenmod km77

# Node
RUN curl -sSL https://nodejs.org/dist/v11.15.0/node-v11.15.0-linux-x64.tar.xz | \
    tar -xJ -C /usr/local --strip-components=1 --exclude='*.md' --exclude=LICENSE

# Install yarn and gulp
RUN npm install -g yarn gulp-cli

# Install Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

WORKDIR /code

#Composer installation
RUN curl -o composer-setup.php https://getcomposer.org/installer && \
    curl -o composer-setup.sig https://composer.github.io/installer.sig && \
    php -r "if (hash('SHA384', file_get_contents('composer-setup.php')) !== trim(file_get_contents('composer-setup.sig'))) { unlink('composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" && \
    php composer-setup.php --version=1.10.16 --quiet && \
    mv composer.phar /usr/local/bin/composer && \
    rm composer-setup.php composer-setup.sig

#APACHE INTALLATION
RUN a2enmod rewrite headers
# Logs to docker logs, as the official php:*-apache image does
RUN ln -sfT /dev/stderr /var/log/apache2/error.log && \
    ln -sfT /dev/stdout /var/log/apache2/access.log && \
    ln -sfT /dev/stdout /var/log/apache2/other_vhosts_access.log

STOPSIGNAL SIGWINCH
EXPOSE 80
CMD ["sh", "-c", ". /etc/apache2/envvars && mkdir -p \"$APACHE_RUN_DIR\" \"$APACHE_LOCK_DIR\" && rm -f \"$APACHE_PID_FILE\" && exec apache2 -DFOREGROUND"]
