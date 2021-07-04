FROM php:7.4.20-fpm
RUN cp "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini" \
    && sed -i '/;date.timezone =/a date.timezone = Asia/Shanghai' "$PHP_INI_DIR/php.ini" \
    && mv /etc/apt/sources.list /etc/apt/sources.list_bak \
    && echo 'deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main' > /etc/apt/sources.list \
    && echo 'deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main' >> /etc/apt/sources.list \
    && echo 'deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main' >> /etc/apt/sources.list \
    && apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
        git \
        unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) bcmath gd pdo_mysql mysqli zip \
    && pecl install redis \
    && docker-php-ext-enable bcmath redis opcache \
    && sed -i '/\[opcache\]/a opcache.enable=1\nopcache.enable_cli=1\nopcache.memory_consumption=128\nopcache.interned_strings_buffer=8\nopcache.max_accelerated_files=10000\nopcache.revalidate_freq=10\nopcache.blacklist_filename=/workbench/conf.d/opcache-blacklist' "$PHP_INI_DIR/php.ini" \
    && curl -O https://getcomposer.org/composer-stable.phar \
    && mv composer-stable.phar /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
