FROM php:7.0-fpm
LABEL maintainer="Son Tran Thanh <286.trants@gmail.com>"

ARG APP_ID=1000

RUN groupadd -g "$APP_ID" app \
  && useradd -g "$APP_ID" -u "$APP_ID" -d /var/www -s /bin/bash app

RUN mkdir -p /etc/nginx/html /var/www /sock \
  && chown -R app:app /etc/nginx /var/www /usr/local/etc/php/conf.d /sock

RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list

RUN curl -fsSL https://deb.nodesource.com/setup_8.x | bash -

RUN apt-get update && apt-get install -y \
  cron \
  git \
  gzip \
  libfreetype6-dev \
  libicu-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libpng-dev \
  libxslt1-dev \
  lsof \
  mysql-client \
  vim \
  zip

RUN docker-php-ext-configure \
  gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN docker-php-ext-install \
  bcmath \
  gd \
  intl \
  mbstring \
  mcrypt \
  opcache \
  pdo_mysql \
  soap \
  xsl \
  zip

RUN pecl channel-update pecl.php.net \
  && pecl install xdebug-2.7.2 \
  && docker-php-ext-enable xdebug \
  && sed -i -e 's/^zend_extension/\;zend_extension/g' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN apt-get install -y libssh2-1-dev \
  && pecl install ssh2-1.1.2 \
  && docker-php-ext-enable ssh2

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

COPY ./conf/blackfire.ini $PHP_INI_DIR/conf.d/blackfire.ini
COPY ./conf/msmtprc /etc/msmtprc
COPY ./conf/php-fpm.conf /usr/local/etc/
COPY ./conf/php.ini $PHP_INI_DIR
COPY ./conf/www.conf /usr/local/etc/php-fpm.d/

USER app:app

VOLUME /var/www

WORKDIR /var/www
