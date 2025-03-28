FROM php:8.1-fpm

RUN apt-get update \
    && apt-get -y --no-install-recommends install \
        libfreetype6-dev \
        libjpeg-dev \
        libpng-dev \
        libwebp-dev \
        libzip-dev \
        libxml2-dev \
        libicu-dev \
        libonig-dev \
        libxslt1-dev \
        zlib1g-dev \
        unzip \
        curl \
        git \
        libssl-dev \
        libldap2-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
        gd \
        intl \
        mysqli \
        pdo_mysql \
        soap \
        zip \
        xsl \
        opcache \
        bcmath \
        ldap \
    # igbinary
    && pecl install igbinary \
    && docker-php-ext-enable igbinary \
    # Redis
    && pecl install -D 'enable-redis-igbinary="yes"' redis \
    && docker-php-ext-enable redis \
    # memcache
    && pecl install memcache \
    && docker-php-ext-enable memcache \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Node.js и NPM
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY ./php.ini /usr/local/etc/php/conf.d/90-php.ini

WORKDIR "/var/www/bitrix"

EXPOSE 9000
