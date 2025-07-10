FROM php:7.4.19-fpm
LABEL maintainer="dev@my.com"

RUN apt-get update \
    && apt-get install -y iputils-ping \
    && apt-get install -y vim \
    && apt-get install -y procps \
    && apt-get install -y cron \
    && apt-get install -y libpng-dev libfreetype6-dev libjpeg62-turbo-dev \
    && apt-get install -y libzip-dev zip \
    && apt-get install -y git \
    && apt-get install -y libcurl4-openssl-dev pkg-config libssl-dev libmemcached-dev libxml2-dev \
    && apt-get install -y supervisor \
    && apt-get install -y strace \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install zip \
	&& docker-php-ext-install gettext \
	&& docker-php-ext-install pcntl \
	&& docker-php-ext-install soap \
	&& docker-php-ext-install mysqli \
	&& docker-php-ext-install calendar \
	&& docker-php-ext-install dom \
	&& docker-php-ext-install intl \
	&& pecl install yaf-3.1.4 && docker-php-ext-enable yaf \
    && pecl channel-update pecl.php.net

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

#install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN git clone https://github.com/xdebug/xdebug.git \
    && cd xdebug \
    && git checkout tags/3.1.6 \
    && phpize \
    && ./configure --enable-xdebug \
    && make \
    && make install

RUN echo "xdebug.remote_enable=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.coverage_enable=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.idekey=\"PHPSTORM\"\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
         "xdebug.remote_port=9001\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN docker-php-ext-enable xdebug

RUN pecl install mongodb-1.16.2 \
    && echo "extension=mongodb.so\n" >> /usr/local/etc/php/conf.d/ext-mongodb.ini

RUN pecl install swoole-4.6.0 \
    && echo "extension=swoole.so\n" >> /usr/local/etc/php/conf.d/ext-swoole.ini

RUN pecl install memcached \
	&& echo "extension=memcached.so\n" >> /usr/local/etc/php/conf.d/ext-memcached.ini

ADD zz-docker.conf /opt
RUN mv /opt/zz-docker.conf /usr/local/etc/php-fpm.d

#设置环境变量
#ENV APOLLO_URL="http://conf.my-test.com"
#ENV APOLLO_TOKEN="68e395d949d609d5ec2f6975a194aaf144903893"
#ENV APOLLO_ENV="DEV"
#ENV APOLLO_SERVER="http://10.128.0.13:8081/"
#ENV APOLLO_CONF_DIR="/opt/apollo-config"

#EXPOSE 9000

#CMD ["php", "-v"]

