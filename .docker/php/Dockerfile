ARG TARGET_PHP_VERSION=7.3
FROM php:${TARGET_PHP_VERSION}-apache

ARG SERVICE_DIR="./workspace"
COPY ./.shared/scripts/ /tmp/scripts/
RUN chmod +x -R /tmp/scripts/

# set timezone
ARG TZ=America/Sao_Paulo
RUN /tmp/scripts/set_timezone.sh ${TZ}

# apache
RUN echo "Include custom.conf" >> /etc/apache2/apache2.conf && a2enmod rewrite
COPY ./.shared/config/apache/custom.conf /etc/apache2/custom.conf

# add users
ARG APP_USER=www-data
ARG APP_GROUP=www-data
ARG APP_USER_ID=1000
ARG APP_GROUP_ID=1000

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN /tmp/scripts/create_user.sh ${APP_USER} ${APP_GROUP} ${APP_USER_ID} ${APP_GROUP_ID}

RUN /tmp/scripts/install_php_extensions.sh

RUN /tmp/scripts/install_software.sh

# php config
COPY ./.shared/config/php/conf.d/*  /usr/local/etc/php/conf.d/

# workdir
ARG APP_CODE_PATH="/var/www/html"
WORKDIR "$APP_CODE_PATH"

# entrypoint
RUN mkdir -p /bin/docker-entrypoint/ \
 && cp /tmp/scripts/docker-entrypoint/* /bin/docker-entrypoint/ \
 && chmod +x -R /bin/docker-entrypoint/ \
;

RUN /tmp/scripts/cleanup.sh

