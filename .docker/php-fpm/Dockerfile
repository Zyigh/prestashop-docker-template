FROM php:7.3.27-fpm-alpine3.13

WORKDIR /

RUN apk update && apk add --no-cache --virtual \
  acl \
  autoconf \
  freetype-dev \
  git \
  g++ \
  ghostscript \
  icu-dev \
  imagemagick \
  imagemagick-dev \
  jpeg-dev \
  libjpeg-turbo-dev \
  libpng-dev \
  libzip-dev \
  make \
  zlib-dev

RUN docker-php-ext-configure gd \
  --with-gd \
  --with-jpeg-dir \
  --with-png-dir \
  --with-zlib-dir \
  --with-freetype-dir

RUN docker-php-ext-install \
  gd \
  intl \
  mbstring \
  mysqli \
  opcache \
  pdo \
  pdo_mysql \
  zip

RUN yes '' | pecl install imagick
RUN docker-php-ext-enable imagick

ADD ./.docker/php-fpm/imagick/ /etc/ImageMagick-6/
ADD ./.docker/php-fpm/conf/www.conf /usr/local/etc/php-fpm.d/www.conf

RUN git clone https://github.com/PrestaShop/PrestaShop.git prestashop

WORKDIR /prestashop
RUN git checkout 1.7.7.2
RUN rm -rf .git

RUN chown -R www-data .

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"
RUN chmod +x /bin/composer

ENV APP_ENV=prod
ENV SYMFONY_ENV=prod

RUN composer install --no-dev --optimize-autoloader
RUN rm /bin/composer

EXPOSE 9000
CMD ["php-fpm", "-F"]
