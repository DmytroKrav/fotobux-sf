FROM composer as composer

FROM php:7.2-fpm

ENV WORK_DIR /var/www/html

RUN apt-get update --fix-missing && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
	    libxml2-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure soap --enable-soap \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) pdo pdo_mysql mbstring dom simplexml soap

COPY --from=composer /usr/bin/composer /usr/local/bin/composer

RUN apt-get -y install \
            git zip \
            libmagickwand-dev \
            --no-install-recommends \
        && pecl install imagick \
        && docker-php-ext-enable imagick \
        && rm -r /var/lib/apt/lists/*

WORKDIR ${WORK_DIR}

CMD ["php-fpm"]
