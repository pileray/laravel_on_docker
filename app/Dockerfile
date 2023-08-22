FROM php:7.4-fpm-alpine
ENV APP_ROOT /var/www/app
WORKDIR $APP_ROOT

RUN apk --update add tzdata && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    apk del tzdata && \
    rm -rf /var/cache/apk/*

RUN apk update && \
    apk upgrade && \
    apk add --update --no-cache \
    autoconf \
    bash \
    build-base \
    curl-dev \
    freetype-dev \
    g++ \
    gcc \
    git \
    libjpeg-turbo-dev \
    libpng-dev \
    libxml2-dev \
    libxslt-dev \
    make \
    mysql-dev \
    mysql-client \
    nginx \
    openssl \
    py3-pip \
    sudo \
    supervisor \
    tzdata \
    vim \
    redis \
    yaml-dev \
    zlib-dev

RUN docker-php-ext-install pdo_mysql soap

RUN docker-php-ext-configure gd \
    --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/ && \
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} gd

RUN pip install awscli

RUN rm -f /usr/local/etc/php-fpm.conf.default
RUN rm -f /usr/local/etc/php-fpm.d/zz-docker.conf
COPY php/php-fpm.conf /usr/local/etc/php-fpm.conf
COPY php/php.ini /usr/local/etc/php/php.ini
COPY php/www.conf /usr/local/etc/php-fpm.d/www.conf

RUN pecl install xdebug-2.9.2 redis
RUN docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --version=2.2.6 --install-dir=/usr/bin
RUN mv /usr/bin/composer.phar /usr/bin/composer

ADD nginx/nginx.conf /etc/nginx/nginx.conf
ADD nginx/dev-laravel.menta.work.conf /etc/nginx/conf.d/dev-laravel.menta.work.conf

COPY supervisor/supervisord.conf /etc/supervisord.conf
COPY supervisor/app.conf /etc/supervisor/conf.d/app.conf
RUN echo files = /etc/supervisor/conf.d/*.conf >> /etc/supervisord.conf

EXPOSE 80
CMD ["/usr/bin/supervisord"]