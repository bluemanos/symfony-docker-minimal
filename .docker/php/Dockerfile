FROM php:8.1-fpm-alpine
ARG TIMEZONE

RUN apk add --no-cache bash libzip libzip-dev libxslt libxslt-dev icu icu-dev oniguruma oniguruma-dev

## TIP: if you want to use GD then comment above line, and uncomment below. Image will increase with ~350MB
#RUN apk add --update --no-cache bash freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev libzip libzip-dev libxslt libxslt-dev icu icu-dev oniguruma oniguruma-dev
#RUN docker-php-ext-configure gd \
#    --with-jpeg \
#    --with-freetype
## In `docker-php-ext-install` you will be able to add `gd`.

RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j$(nproc) pdo pdo_mysql zip xsl intl opcache mbstring

RUN apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev libzip-dev libxslt-dev icu-dev oniguruma-dev

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
    && "date"

# Install composer and symfony cli
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -sS https://get.symfony.com/cli/installer | bash \
    && mv /root/.symfony5/bin/symfony /usr/local/bin/symfony \
    && echo 'alias sf="php bin/console"' >> ~/.bashrc

WORKDIR /var/www