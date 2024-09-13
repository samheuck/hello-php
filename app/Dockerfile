FROM public.ecr.aws/amazonlinux/amazonlinux:2023
RUN yum check-update && yum update -y && yum install -y \
  fontconfig \
  freetype \
  libX11 \
  libXext \
  libXrender \
  libjpeg \
  libpng \
  openssl \
  xorg-x11-fonts-75dpi \
  xorg-x11-fonts-Type1 \
  https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox-0.12.6.1-3.almalinux9.$(uname -m).rpm \
  dejavu-fonts-all \
  php8.3 \
  php8.3-devel \
  php-pear \
  php8.3-zip \
  php8.3-pgsql \
  php8.3-fpm \
  php8.3-intl \
  && fc-cache -f

RUN pecl install igbinary \
  && pecl install --configureoptions 'enable-redis-igbinary="yes" enable-redis-lzf="yes" enable-redis-zstd="no"' redis-5.3.7

RUN ln -s /usr/local/bin/wkhtmltopdf /usr/bin/wkhtmltopdf \
  && ln -s /usr/local/bin/wkhtmltoimage /usr/bin/wkhtmltoimage

RUN mkdir -p /run/php-fpm

COPY php-fpm.conf /etc/php-fpm.d/www.conf
COPY 10-opcache.ini /etc/php.d/10-opcache.ini
COPY 99-php-custom.ini /etc/php.d/99-php-custom.ini

COPY src /var/www/html/

WORKDIR /var/www/html
EXPOSE 9000
CMD ["/bin/bash", "-c", "php-fpm -F"]
