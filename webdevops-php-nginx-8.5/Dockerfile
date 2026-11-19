FROM webdevops/php-nginx:8.5@sha256:1987cec13616f5b6ad6aceca97ae68f14f0573c9f42934b5d179cfe0537bea47

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
