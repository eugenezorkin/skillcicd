FROM php:8.3-fpm

# Устанавливаем нужный UID/GID для www-data, чтобы совпадал с локальным пользователем
ARG WWW_UID=1000
ARG WWW_GID=1000

RUN groupmod -g $WWW_GID www-data && \
    usermod -u $WWW_UID -g www-data www-data

# Устанавливаем необходимые зависимости и PHP-расширения
RUN apt-get update && apt-get install -y \
    git unzip curl libpq-dev librabbitmq-dev libzip-dev zip \
    libicu-dev libonig-dev libxml2-dev telnet \
    libpng-dev libjpeg-dev libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
       gd mysqli pdo pdo_mysql pdo_pgsql zip intl opcache bcmath pcntl \
    && docker-php-ext-enable opcache \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Устанавливаем Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Указываем рабочую директорию
WORKDIR /var/www/html

# Устанавливаем права на запись в var/
# Создаём директории и даём права на запись
RUN mkdir -p var public /var/www/.composer \
    && chown -R www-data:www-data /var/www/html /var/www/.composer  \
    && chmod -R 775 var public /var/www/.composer

# Меняем пользователя контейнера
USER www-data
