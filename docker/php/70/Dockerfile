FROM node:14.15.1-slim AS node
FROM php:7.0-fpm

COPY --from=node /usr/local/bin /usr/local/bin
COPY --from=node /usr/local/lib /usr/local/lib
COPY --from=node /opt /opt
COPY --from=composer:1.7.1 /usr/bin/composer /usr/bin/composer

RUN apt-get update

RUN apt-get install -y --no-install-recommends \
        wget curl zip git zsh vim \
        apt-utils \
        libgmp-dev \
        libbz2-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libfreetype6-dev \
        libgmp-dev \
        libmagickwand-dev \
        libmagickcore-dev \
        libxslt1-dev \
        libyaml-dev \
        libzip-dev \
        libonig-dev \
        libmcrypt-dev \
        libxslt-dev

RUN pecl install xdebug-2.6.0

RUN docker-php-ext-enable xdebug

RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include --with-jpeg-dir=/usr/include

RUN docker-php-ext-configure zip
RUN docker-php-ext-configure intl

RUN docker-php-ext-install -j$(nproc) \
    bcmath \
    bz2 \
    exif \
    gettext \
    gmp \
    intl \
    zip \
    soap \
    mysqli \
    gd \
    mbstring \
    pdo_mysql \
    sockets \
    mcrypt \
    xsl \
    xmlrpc

COPY etc/infrastructure/php/ /usr/local/etc/php/

RUN usermod -u 1000 www-data
RUN chsh -s /bin/zsh www-data

RUN wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh
RUN sh ./install.sh
RUN rm ./install.sh
