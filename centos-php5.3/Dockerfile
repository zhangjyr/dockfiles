# DOCKER-VERSION 1.0.0

FROM    zhangjyr/supervisord:centos6.6

MAINTAINER Jingyuan Zhang, zhang@jingyuan.name

# Step1 Install php
RUN yum -y install "freetype" "libjpeg" "libpng" "libmcrypt" "openssl" "mysql-libs" "gcc" "freetype-devel" "libjpeg-devel" "libpng-devel" "libmcrypt-devel" "libxml2-devel" "openssl-devel" "curl-devel"
ADD php-5.3.29.tar.gz /
RUN mv /php-5.3.29 /php && cd /php && ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --enable-fpm --enable-inline-optimization --disable-fileinfo --enable-bcmath --enable-mbstring --enable-sockets --enable-zip --with-gd --enable-gd-native-ttf --with-curl --with-mcrypt --with-freetype-dir --with-jpeg_dir --with-png-dir --with-openssl --disable-debug --disable-maintainer-zts && make && make install && cd /
RUN rm -rf /php && yum -y erase "freetype-devel" "libjpeg-devel" "libpng-devel" "libmcrypt-devel" "libxml2-devel" "openssl-devel" "curl-devel"

# Step2 Add php-fpm configuration
ADD php-fpm.conf /usr/local/php/etc/php-fpm.conf
ADD scripts/php-fpm /etc/init.d/php-fpm
RUN ln -s /usr/local/php/sbin/php-fpm /usr/sbin/php-fpm

# Step3 Override supervisord configuration
ADD supervisord.conf /etc/supervisord.conf

RUN groupadd www && useradd www -g www

EXPOSE 9000
