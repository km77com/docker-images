FROM webdevops/php-nginx:7.4

# Node
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash - && apt-get install -y nodejs

# Install yarn
RUN apt-get install -y apt-transport-https
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y yarn

# Install gulp
RUN npm install gulp-cli -g
RUN npm install gulp -D

# Install Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

WORKDIR /code
