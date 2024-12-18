FROM php:7.3-fpm

RUN apt-get update && apt-get install -qqy git unzip \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        libpng-dev \
        libzip-dev \
        libaio1 \
        g++ gcc curl \
        nano htop \
        wget && apt-get clean autoclean && apt-get autoremove --yes &&  rm -rf /var/lib/{apt,dpkg,cache,log}/

# enable gd and zip extension
RUN docker-php-ext-install gd
RUN docker-php-ext-install zip

RUN docker-php-ext-install pdo pdo_mysql
RUN docker-php-ext-enable pdo_mysql

ADD https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-basic-linux.x64-21.1.0.0.0.zip /tmp/
ADD https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-sdk-linux.x64-21.1.0.0.0.zip /tmp/
ADD https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-sqlplus-linux.x64-21.1.0.0.0.zip /tmp/

# unzip them
RUN unzip /tmp/instantclient-basic-linux.x64-*.zip -d /usr/local/ \
    && unzip /tmp/instantclient-sdk-linux.x64-*.zip -d /usr/local/ \
    && unzip /tmp/instantclient-sqlplus-linux.x64-*.zip -d /usr/local/

# install oci8
RUN ln -s /usr/local/instantclient_*_1 /usr/local/instantclient \
    && ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus

RUN docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient \
    && docker-php-ext-install oci8 \
    && echo /usr/local/instantclient/ > /etc/ld.so.conf.d/oracle-insantclient.conf \
    && ldconfig

RUN echo "instantclient,/usr/local/instantclient_21_1" | pecl install oci8-2.2.0

# Add UID '1000' to www-data
RUN usermod -u 1000 www-data

# Download Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer