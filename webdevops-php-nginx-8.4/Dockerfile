FROM webdevops/php-nginx:8.4@sha256:99aa1f11de058c8217e7a84efb02de9923fb73a181a9f0221ec270a26d97810b

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
