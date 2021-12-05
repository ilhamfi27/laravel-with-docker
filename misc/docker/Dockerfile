FROM php:7.4-fpm-alpine as base

# Install system dependencies
RUN apk --update add wget \
    curl \
    git \
    grep \
    build-base \
    libmemcached-dev \
    libmcrypt-dev \
    libxml2-dev \
    imagemagick-dev \
    oniguruma \
    oniguruma-dev \
    pcre-dev \
    libtool \
    make \
    autoconf \
    g++ \
    cyrus-sasl-dev \
    libgsasl-dev \
    freetype \
    libjpeg-turbo \
    libpng \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev

# Installing bash
RUN apk add bash
RUN sed -i 's/bin\/ash/bin\/bash/g' /etc/passwd

# Install PHP extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql mbstring exif pcntl bcmath gd
RUN pecl channel-update pecl.php.net \
    && pecl install memcached \
    && pecl install imagick \
    && docker-php-ext-enable memcached \
    && docker-php-ext-enable imagick \
    && docker-php-ext-enable gd

# Clear cache
RUN rm /var/cache/apk/*
RUN rm -rf /tmp/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

FROM php:7.4-fpm-alpine as builder

# Install system dependencies
RUN apk --update add wget \
    curl \
    git \
    grep \
    build-base \
    libmemcached-dev \
    libmcrypt-dev \
    libxml2-dev \
    imagemagick-dev \
    oniguruma \
    oniguruma-dev \
    pcre-dev \
    libtool \
    make \
    autoconf \
    g++ \
    cyrus-sasl-dev \
    libgsasl-dev \
    freetype \
    libjpeg-turbo \
    libpng \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev

# Installing bash
RUN apk add bash
RUN sed -i 's/bin\/ash/bin\/bash/g' /etc/passwd

# Install PHP extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql mbstring exif pcntl bcmath gd
RUN pecl channel-update pecl.php.net \
    && pecl install memcached \
    && pecl install imagick \
    && docker-php-ext-enable memcached \
    && docker-php-ext-enable imagick \
    && docker-php-ext-enable gd

# Clear cache
RUN rm /var/cache/apk/*
RUN rm -rf /tmp/*

COPY --from=base /usr/local/bin/composer /usr/local/bin/composer

WORKDIR /var/www

COPY . .

RUN chmod 777 -R storage

RUN composer install

ENTRYPOINT [ "/bin/bash", "-c" ]
