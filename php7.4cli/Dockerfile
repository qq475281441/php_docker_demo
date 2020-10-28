FROM php:7.4-cli

COPY sources.list .

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak&&mv sources.list /etc/apt/&&apt-get update -y

RUN apt-get install -y libonig-dev \
        libxml2 \
        libxml2-dev \
        libmcrypt-dev \
        libzip-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev

RUN docker-php-ext-configure bcmath --enable-bcmath \
    && docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql \
    && docker-php-ext-configure mbstring --enable-mbstring \
    && docker-php-ext-configure soap --enable-soap \
    && docker-php-ext-install \
        bcmath \
        intl \
        mbstring \
        mysqli \
        pcntl \
        pdo_mysql \
        soap \
        sockets \
        zip \
        opcache \
  && docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd

#COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

RUN pecl install redis mcrypt \
    && pecl install xdebug \
    && pecl install swoole-4.5.3 \
    && docker-php-ext-enable redis xdebug swoole mcrypt

RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && php composer-setup.php &&\
       php -r "unlink('composer-setup.php');"

#COPY ../../../ /www
#WORKDIR /www
#CMD [ "php", "think swoole start" ]