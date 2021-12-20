FROM php:8.0.14-apache-bullseye

ENV ACCEPT_EULA=Y
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mysqli
RUN apt-get update && apt-get install -y gnupg2 apt-transport-https wget
RUN apt-get update
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - 
RUN curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get install -y --no-install-recommends locales
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen
RUN locale-gen
RUN apt-get update 
RUN apt-get install -y unixodbc 
RUN apt-get install -y unixodbc-dev 
RUN apt-get install -y odbcinst 
RUN apt-get install -y locales
RUN apt-get install -y libgss3 
RUN TEMP_DEB="$(mktemp)" \
  && wget -O "$TEMP_DEB" 'https://packages.microsoft.com/debian/10/prod/pool/main/m/msodbcsql17/msodbcsql17_17.8.1.1-1_amd64.deb' \
  && yes | dpkg --skip-same-version -i "$TEMP_DEB" \
  && rm -f "$TEMP_DEB"
RUN apt-get install -y msodbcsql17 
RUN pecl install xdebug
RUN pecl install sqlsrv
RUN pecl install pdo_sqlsrv
RUN docker-php-ext-enable xdebug
RUN docker-php-ext-enable sqlsrv
RUN docker-php-ext-enable pdo_sqlsrv
RUN echo "zend_extension=xdebug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
  && echo " " >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
  && echo "[xdebug]" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
  && echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
  && echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
  && echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
  && echo "; xdebug.log=/var/www/html/xdebug.log" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini