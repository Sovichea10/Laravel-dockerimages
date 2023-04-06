FROM ubuntu:20.04

RUN apt-get update && apt-get install -y \
        nano \
        htop \
        libxml2-dev \
        software-properties-common \
        && add-apt-repository -y ppa:ondrej/php \
        && apt-get update && apt-get install -y \
        php8.1-fpm \
        php8.1-soap \
        php8.1-xml \
        build-essential \
        libmcrypt-dev \
        git unzip libfreetype6-dev \
        libjpeg-dev \
        libaio1 wget && apt-get clean autoclean && apt-get autoremove --yes &&  rm -rf /var/lib/{apt,dpkg,cache,log}

# install nvm
RUN apt-get update && apt-get install -y \
    curl dirmngr apt-transport-https lsb-release ca-certificates

RUN curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    # source ~/.bashrc

# Download composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Download Oracle Instant Client
ADD https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-basic-linux.x64-21.1.0.0.0.zip /tmp/
ADD https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-sdk-linux.x64-21.1.0.0.0.zip /tmp/
ADD https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-sqlplus-linux.x64-21.1.0.0.0.zip /tmp/

# unzip oracle
RUN unzip /tmp/instantclient-basic-linux.x64-*.zip -d /usr/local/ \
    && unzip /tmp/instantclient-sdk-linux.x64-*.zip -d /usr/local/ \
    && unzip /tmp/instantclient-sqlplus-linux.x64-*.zip -d /usr/local/

# linker
RUN ln -s /usr/local/instantclient_*_1 /usr/local/instantclient \
    && ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus && \
    echo /usr/local/instantclient/ > /etc/ld.so.conf.d/oracle-instantclient.conf && \
    ldconfig

# install pecl
RUN apt-get update && apt-get install -y \
        php8.1-dev \
        php-pear

# install php-curl extension
RUN apt-get update && apt-get install -y php8.1-curl

# install oci8 extension
RUN pecl channel-update pecl.php.net && \
    echo "instantclient,/usr/local/instantclient_21_1" | pecl install oci8 && \
    echo "extension=oci8.so" >> /etc/php/8.1/cli/php.ini && \
    echo "extension=oci8.so" >> /etc/php/8.1/mods-available/oci8.ini && \
    ln -s /etc/php/8.1/mods-available/oci8.ini /etc/php/8.1/cli/conf.d/30-oci8.ini

# install mysql extension
RUN apt-get update && apt-get install -y php8.1-mysql

# php -m | grep <name>
# php -i | grep <name>

# Restart service php-fpm
RUN service php8.1-fpm restart