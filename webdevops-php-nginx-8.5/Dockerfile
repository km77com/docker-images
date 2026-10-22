# Pinned by digest: PHP 8.5.12, with the fix for CVE-2026-109618 and CVE-2026-109619
FROM webdevops/php-nginx:8.5@sha256:e63f305ed44cd977a1e2cf92d4d5d3247eccefc8351856bca5745118cb5e71d3

# Node
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
