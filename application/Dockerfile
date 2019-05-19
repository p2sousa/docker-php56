FROM  php:5.6-fpm

MAINTAINER Pablo Sousa <pablosousa.ads@gmail.com>

# ImageMagick
RUN apt-get update && apt-get install -y libmagickwand-6.q16-dev --no-install-recommends \
  && ln -s /usr/lib/x86_64-linux-gnu/ImageMagick-6.8.9/bin-Q16/MagickWand-config /usr/bin \
  && pecl install imagick \
  && echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini

# PHP & Utils
RUN apt-get update && apt-get install -y --no-install-recommends \
  vim \
  git \
  unzip \
  cron \
  libcurl3-dev \
  zlib1g-dev \
  libicu-dev \
  && apt-get clean; rm -rf /var/lib/apt/lists/* \
  && docker-php-ext-install curl intl json mbstring mysql mysqli pdo pdo_mysql opcache zip fileinfo\
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install gd

RUN pecl install apcu-4.0.11 \
    && docker-php-ext-enable apcu

ARG CODE_ENV
ARG GITHUB_OAUTH
ARG UID=1000

# Set timezone to America/Sao_Paulo
RUN rm /etc/localtime \
  && ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

# Copy .ini config file
COPY ./app.ini /usr/local/etc/php/conf.d/

# Create a user
RUN useradd --uid ${UID:-1000} --no-user-group -g www-data \
    --create-home --shell /bin/false psousa

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
  --install-dir=/usr/local/bin --filename=composer


# Environments
ENV HOME=/home/psousa
ENV APP=/home/psousa/projects

# Create the 'application' directories
RUN mkdir -p $APP

# Copy application folder and set up its permissions
COPY ./projects/ $APP/

USER root

# Update ownership of home folder with the correct user and group
RUN chown -R psousa:www-data $HOME/*

RUN chown -R psousa:www-data $APP/*

# Copy entrypoint file
COPY ./entrypoint.sh /tmp/entrypoint.sh
RUN chmod 0777 /tmp/entrypoint.sh

# Copy init_database, which further will create the schema and load the fixtures
COPY ./init_database.sh /tmp/init_database.sh

## Add write permission to whole tmp for file uploads
RUN chmod -R 0777 /tmp

USER psousa
WORKDIR $APP
VOLUME $APP

ENTRYPOINT ["/tmp/entrypoint.sh"]
