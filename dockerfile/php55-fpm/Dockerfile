FROM php:5.5.38-fpm

LABEL maintainer="dev@my.com"

RUN apt-get update \
    && apt-get install -y iputils-ping \
    && apt-get install -y vim \
    && apt-get install -y procps \
    && apt-get install -y cron \
    && apt-get install -y libpng-dev libfreetype6-dev libjpeg62-turbo-dev \
    && apt-get install -y libzip-dev zip \
    && apt-get install -y git \
    && apt-get install -y libcurl4-openssl-dev pkg-config libssl-dev \
    && apt-get install -y strace \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install mysql \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip \
    && pecl channel-update pecl.php.net

#RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

#install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN git clone https://github.com/xdebug/xdebug.git \
    && cd xdebug \
    && git checkout tags/2.3.0 \
    && phpize \
    && ./configure --enable-xdebug \
    && make \
    && make install

RUN echo "xdebug.remote_enable=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.coverage_enable=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.idekey=\"PHPSTORM\"\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.remote_port=9001\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN docker-php-ext-enable xdebug

#设置环境变量
#ENV APOLLO_URL="http://conf.my-test.com"
#ENV APOLLO_TOKEN="68e395d949d609d5ec2f6975a194aaf144903893"
#ENV APOLLO_ENV="DEV"
#ENV APOLLO_SERVER="http://10.128.0.13:8081/"
#ENV APOLLO_CONF_DIR="/opt/apollo-config"

#EXPOSE 9000

#CMD ["php", "-v"]

