# Start with the official PHP 7.4 image
FROM php:7.4-apache

# Install system dependencies and clean cache
RUN apt-get update && apt-get install -y \
 git \
 curl \
 libpng-dev \
 libonig-dev \
 libxml2-dev \
 libpng-dev \
 libjpeg-dev \
 libwebp-dev \
 zip \
 unzip \
 libzip-dev

# Install PHP extensions
RUN docker-php-ext-configure gd --enable-gd --with-jpeg --with-webp
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath zip gd

# Copy Composer from the official Composer image
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN chmod +x /usr/bin/composer

WORKDIR /var/www/html

COPY .docker/phpconfig.ini /usr/local/etc/php/conf.d/phpconfig.ini

# Run Composer Install, needs to copy the server but is overwritten by the volume mapping in docker-compose
COPY server/composer.json server/composer.lock ./
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN composer install --no-scripts --no-autoloader --no-interaction
COPY server .
RUN composer dump-autoload --optimize && composer run-script post-install-cmd

# Configure Apache
RUN a2dissite 000-default.conf
COPY .docker/000-default.conf /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite
RUN a2ensite 000-default.conf