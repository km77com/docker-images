# Pinned by digest: PHP 8.4.27, with the fix for CVE-2026-109618 and CVE-2026-109619
FROM webdevops/php-nginx:8.4@sha256:9d23e38bb909caf91653d3c77bf611fdf1fe9d4da2e19404acf624f9ae38b640

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
