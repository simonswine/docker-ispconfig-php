# Based on ubuntu 12.04
FROM ubuntu:precise

MAINTAINER Christian Simon <simon@swine.de>

ENV PHP_VERSION 4.4.9

# Update everything & install prequesites
RUN apt-get update &&  \
    DEBIAN_FRONTEND=noninteractive apt-get -y install build-essential wget bzip2 \
        libpng-dev libmcrypt-dev libmcrypt4 libmhash-dev libmysqlclient-dev \
        libjpeg-dev zlib1g-dev libfreetype6-dev libfontconfig1-dev \
        libgpg-error-dev uuid-dev libt1-dev libsnmp-dev libc-client2007e-dev \
        libaspell-dev libbz2-dev libc-client2007e-dev flex bison libsqlite3-dev libpq-dev \
    libcurl4-openssl-dev libmagickwand-dev libgd2-xpm-dev && \
    DEBIAN_FRONTEND=noninteractive apt-get -y dist-upgrade && \
    apt-get clean && \
    rm /var/lib/apt/lists/*_*

# Hotfixes php-4.4
## Install libdb4.6 from lucid
RUN mkdir -p /tmp/install/ && \
    cd /tmp/install && \
    wget http://de.archive.ubuntu.com/ubuntu/pool/universe/d/db4.6/libdb4.6_4.6.21-16_amd64.deb && \
    wget http://de.archive.ubuntu.com/ubuntu/pool/universe/d/db4.6/libdb4.6-dev_4.6.21-16_amd64.deb && \
    echo "2f03a50d72f66d6c6ac976cb0ff1131a  libdb4.6-dev_4.6.21-16_amd64.deb" > md5sums && \
    echo "3f4d7da181ad71d89011983019687929  libdb4.6_4.6.21-16_amd64.deb" >> md5sums && \
    md5sum -c md5sums && \
    dpkg -i libdb4.6-dev_4.6.21-16_amd64.deb libdb4.6_4.6.21-16_amd64.deb && \
    cd && \
    rm -rf /tmp/install

## Install compatible openssl 0.9.8
RUN mkdir -p /tmp/install/ && \
    cd /tmp/install && \
    wget http://www.openssl.org/source/openssl-0.9.8za.tar.gz && \
    echo "2f989915f8fea49aa1bc37aa58500cce  openssl-0.9.8za.tar.gz" > md5sums && \
    md5sum -c md5sums && \
    tar xfz openssl-0.9.8za.tar.gz && \
    cd openssl-0.9.8za && \
    ./config --prefix=/usr/local --openssldir=/usr/local/openssl && \
    make && \
    make test && \
    make install && \
    cd && \
    rm -rf /tmp/install

## Symlink libraries
RUN for i in /usr/lib/x86_64-linux-gnu/*.so ; do f=`basename $i`;ln -sf x86_64-linux-gnu/$f /usr/lib/$f; done
RUN for i in /usr/lib/x86_64-linux-gnu/*.a ; do f=`basename $i`;ln -sf x86_64-linux-gnu/$f /usr/lib/$f; done

# Download PHP
RUN mkdir -p /tmp/install/ && \
    cd /tmp/install && \
    wget http://museum.php.net/php4/php-${PHP_VERSION}.tar.bz2 && \
    echo "2e3b2a0e27f10cb84fd00e5ecd7a1880  php-4.4.9.tar.bz2" > md5sums && \
    md5sum -c md5sums && \
    tar xfj php-${PHP_VERSION}.tar.bz2 && \
    cd php-${PHP_VERSION} && \
    ./configure --enable-cgi \
        --with-libdir=lib/x86_64-linux-gnu \
        --with-mcrypt  \
        --with-zlib \
        --with-curl \
        --disable-debug \
        --disable-rpath \
        --enable-inline-optimization \
        --with-bz2 \
        --with-zlib \
        --enable-sockets \
        --enable-sysvsem  \
        --enable-sysvshm \
        --enable-pcntl \
        --enable-mbregex \
        --with-mhash \
        --enable-zip  \
        --with-pcre-regex \
        --with-mysqli  \
        --with-mysql \
        --with-gd=shared,/usr  \
        --enable-gd-native-ttf  \
        --with-ldap \
        --with-mcrypt \
        --with-mhash   \
        --with-snmp \
        --enable-ctype \
        --with-freetype-dir=shared,/usr  \
        --with-jpeg-dir=/usr  \
        --with-t1lib=/usr  \
        --enable-bcmath \
        --with-bz2 \
        --enable-ctype \
        --with-db4 \
        --with-iconv  \
        --enable-exif \
        --enable-ftp \
        --with-gettext \
        --enable-mbstring \
        --with-imap \
        --with-imap-ssl  \
        --with-kerberos \
        --with-openssl \
        --enable-intl \
        --with-pgsql \
        --with-pdo-mysql  \
        --enable-soap  \
        --with-tidy \
        --with-libxml-dir=/usr  \
        --with-openssl  \
        --with-openssl-dir=/usr/local  \
        --with-xsl && \
    make && \
    make install
 
