FROM zhangjyr/php:5.4

ADD src/* /usr/src/php/ext/
#COPY docker-php-ext-*.ini /usr/local/etc/php/conf.d/

RUN docker-php-ext-install igbinary-1.2.1

RUN buildDeps=" \
                    cyrus-sasl-devel \
    " \
    && set -x \
    && yum -y install gcc-c++ $buildDeps \
    && cd /usr/src/php/ext/libmemcached-1.0.18 \
    && ./configure \
    && make && make install && make clean \
    && docker-php-ext-install igbinary-1.2.1 \
    && docker-php-ext-configure memcached-2.2.0 --enable-memcached-igbinary \
    && docker-php-ext-install memcached-2.2.0 \
    && yum -y erase $buildDeps

RUN docker-php-ext-install phpredis-2.2.7
