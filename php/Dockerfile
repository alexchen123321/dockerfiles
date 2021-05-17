FROM php:7.3-fpm-alpine

WORKDIR /home/wwwroot

# Install dependencies
RUN apk update && apk add --no-cache \
    $PHPIZE_DEPS \
    bash \
    build-base shadow vim curl \
    git \
    php7 \
    php7-fpm \
    php7-common \
    php7-pdo \
    php7-pdo_mysql \
    php7-mysqli \
    php7-mcrypt \
    php7-mbstring \
    php7-xml \
    php7-openssl \
    php7-json \
    php7-phar \
    php7-zip \
    php7-gd \
    php7-dom \
    php7-session \
    php7-zlib \
    zip libzip-dev

# Add and Enable PHP-PDO Extenstions
RUN docker-php-ext-install pdo_mysql

RUN pecl install redis-5.1.1 \
    && pecl install xdebug-2.8.1 \
    && docker-php-ext-enable redis xdebug

RUN docker-php-ext-install zip exif

# Add gd extension
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev && \
  docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} gd && \
  apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

# Add php.ini
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini 

# Install Composer
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN wget https://getcomposer.org/download/1.10.20/composer.phar && mv ./composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer

# Add nodejs and npm
RUN apk add --update nodejs npm

# Remove Cache
RUN rm -rf /var/cache/apk/*

# Add user for laravel application
RUN groupadd -g 1000 www && useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory permissions
COPY --chown=www:www . /home/wwwroot

# replace www-data in php conf
RUN sed -i  "s/www-data/www/g" /usr/local/etc/php-fpm.d/www.conf

# Change current user to www
USER root

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
