FROM daocloud.io/library/php:7.0.8-fpm

ADD php.ini    /usr/local/etc/php/php.ini

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libmemcached-dev \
        git \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd pdo_mysql mysqli zip exif


# Install Memcached for php 7
RUN curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
    && mkdir -p /usr/src/php/ext/memcached \
    && tar -C /usr/src/php/ext/memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
    && echo 'memcached' >> /usr/src/php-available-exts \
    && docker-php-ext-install memcached \
    && rm /tmp/memcached.tar.gz

ENV PHPREDIS_VERSION 3.0.0

RUN curl -L -o /tmp/redis.tar.gz "https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz" \
    && tar xzf /tmp/redis.tar.gz -C /tmp \
    && mkdir -p /usr/src/php/ext \
    && rm -r /tmp/redis.tar.gz \
    && mv /tmp/phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
    && echo 'redis' >> /usr/src/php-available-exts \
    && docker-php-ext-install redis \
    && rm -rf /usr/src/php/ext/redis
