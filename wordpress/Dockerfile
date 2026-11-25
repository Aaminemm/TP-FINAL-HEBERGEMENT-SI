FROM php:8.4-apache

ARG WORDPRESS_VERSION=7.1.5
ARG WORDPRESS_SHA1=f79cd80a8f8152edd88ef9be859ae765f2ec6c36

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      libfreetype6-dev libicu-dev libjpeg62-turbo-dev libpng-dev libwebp-dev libzip-dev \
 && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
 && docker-php-ext-install -j"$(nproc)" exif gd intl mysqli opcache zip \
 && a2enmod rewrite \
 && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL -o /tmp/wordpress.tar.gz "https://wordpress.org/wordpress-${WORDPRESS_VERSION}.tar.gz" \
 && echo "${WORDPRESS_SHA1}  /tmp/wordpress.tar.gz" | sha1sum -c - \
 && tar xzf /tmp/wordpress.tar.gz -C /var/www/html --strip-components=1 --no-same-owner \
 && rm /tmp/wordpress.tar.gz \
 && mkdir -p /var/www/html/wp-content/uploads \
 && chown -R www-data:www-data /var/www/html/wp-content

COPY --chmod=644 wp-config.php .htaccess /var/www/html/

EXPOSE 80
