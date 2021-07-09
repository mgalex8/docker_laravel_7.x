FROM php:7.4-fpm-alpine3.13

# Copy File Config
ADD ./compose/php/www.conf /usr/local/etc/php-fpm.d/www.conf

# ADD and set Group
RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

# Create folder to run
RUN mkdir -p /var/www/html

# Set Profile
RUN chown laravel:laravel /var/www/html

# Work in the specific space
WORKDIR /var/www/html

# Install dependencies
RUN apk add --no-cache \
    freetype \
    libpng \
    libjpeg-turbo \
    freetype-dev \
    libpng-dev \
    libjpeg-turbo-dev

RUN docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg

RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} gd
#RUN docker-php-ext-configure gd --with-jpeg --with-freetype \
#	&& docker-php-ext-install gd

RUN apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

RUN docker-php-ext-install pdo pdo_mysql

# install and enable xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
	&& pecl install xdebug \
	&& docker-php-ext-enable xdebug


#FROM php:7.4-fpm
#
#RUN apt-get update
#RUN apt-get install -y \
#     libfreetype6-dev \
#     libjpeg62-turbo-dev \
#     libmcrypt-dev \
#     libpng-dev \
#     zlib1g-dev \
#     libxml2-dev \
#     libzip-dev \
#     libonig-dev \
#     graphviz \
#     libcurl4-openssl-dev \
#     pkg-config \
#     libpq-dev
#
#RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
#  && docker-php-ext-install -j$(nproc) gd \
#  && docker-php-source delete
#
##RUN apt-get delete freetype-dev libpng-dev libjpeg-turbo-dev
#
#RUN docker-php-ext-install pdo pdo_mysql
#
### install and enable xdebug
#RUN pecl install xdebug \
#    && docker-php-ext-enable xdebug
#
#RUN addgroup --gid 1000 laravel
#    && adduser --ingroup laravel --gid laravel --shell /bin/sh -D laravel