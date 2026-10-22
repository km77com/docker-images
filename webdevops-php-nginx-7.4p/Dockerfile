FROM webdevops/php-nginx:7.4 AS php-build

# buster is EOL: its apt repos moved to archive.debian.org; the image's nginx.org signing key has expired
RUN sed -ri 's#//(deb|security)\.debian\.org/#//archive.debian.org/#' /etc/apt/sources.list && \
    curl -sS https://nginx.org/keys/nginx_signing.key | apt-key add -

# Rebuild the image's own PHP sources with the fix for CVE-2026-109618 and CVE-2026-109619
# (minimal backport for EOL versions, from php-src branch PHP-7.4-CVE-2026-109618-109619), same configure flags.
# etc/ is dropped: it only has *.default files and copying it would replace the php-fpm.d symlink to webdevops' pools
COPY cve-2026-109618-109619.patch /usr/src/
RUN apt-get update && apt-get install -y $PHPIZE_DEPS libargon2-dev libcurl4-openssl-dev libonig-dev libreadline-dev \
        libsodium-dev libsqlite3-dev libssl-dev libxml2-dev zlib1g-dev && \
    docker-php-source extract && cd /usr/src/php && \
    patch -p1 < /usr/src/cve-2026-109618-109619.patch && \
    CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
        ./configure $(php-config --configure-options) --without-pear && \
    make -j"$(nproc)" && \
    make install INSTALL_ROOT=/php && \
    strip --strip-all /php/usr/local/bin/php /php/usr/local/sbin/php-fpm && \
    find /php -type f -name '*.so' -exec strip --strip-all '{}' + && \
    rm -r /php/usr/local/etc

FROM webdevops/php-nginx:7.4

COPY --from=php-build /php/usr/local/ /usr/local/

# buster is EOL: its apt repos moved to archive.debian.org; the image's nginx.org signing key has expired
RUN sed -ri 's#//(deb|security)\.debian\.org/#//archive.debian.org/#' /etc/apt/sources.list && \
    curl -sS https://nginx.org/keys/nginx_signing.key | apt-key add -

# Node (the nodesource node_11.x repo no longer verifies)
RUN curl -sSL https://nodejs.org/dist/v11.15.0/node-v11.15.0-linux-x64.tar.xz | \
    tar -xJ -C /usr/local --strip-components=1 --exclude='*.md' --exclude=LICENSE

# Install yarn
RUN npm install -g yarn

# Install gulp
RUN npm install gulp-cli -g
RUN npm install gulp -D

# Install Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

WORKDIR /code
