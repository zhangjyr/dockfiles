# DOCKER-VERSION 1.0.0

FROM    centos:6.6

MAINTAINER Jingyuan Zhang, zhang@jingyuan.name

# Step1 Install dependencies for HHVM
# yum update -y >/dev/null && 
RUN yum install -y http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm  && curl -L -o /etc/yum.repos.d/hop5.repo "http://www.hop5.in/yum/el6/hop5.repo"

# Step2 Install supervisor
RUN yum install -y python-setuptools && easy_install supervisor

# Step3-7 Install php
RUN yum -y install "freetype" "libjpeg" "libpng" "libmcrypt" "openssl" "mysql-libs" "gcc" "freetype-devel" "libjpeg-devel" "libpng-devel" "libmcrypt-devel" "libxml2-devel" "openssl-devel" "curl-devel"
ADD php-5.4.38.tar.gz /
RUN mv /php-5.4.38 /php && cd /php && ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --enable-fpm --enable-inline-optimization --disable-fileinfo --enable-bcmath --enable-mbstring --enable-sockets --enable-zip --with-gd --enable-gd-native-ttf --with-curl --with-mcrypt --with-freetype-dir --with-jpeg_dir --with-png-dir --with-openssl --disable-debug --disable-maintainer-zts && make && make install && cd /
RUN rm -rf /php && yum -y erase "freetype-devel" "libjpeg-devel" "libpng-devel" "libmcrypt-devel" "libxml2-devel" "openssl-devel" "curl-devel"

# Step8 install nginx, php, mysql, hhvm
RUN ["yum", "-y", "install", "nginx", "hhvm"]

# Step9 Create folder for server and add index.php file to for nginx
RUN mkdir -p /web/online && chmod a+r /web/online && echo "<?php echo 'Rerun image with option -v localpath:/web/online to view your site.<br/>';phpinfo(); ?>" > /web/online/index.php

# Step10-11 Setup hhvm - add config for hhvm
ADD hhvm.hdf /etc/hhvm/config.hdf 

RUN service hhvm restart

# Step12 ADD Nginx config
ADD nginx.conf /etc/nginx/conf.d/default.conf

# Step13 ADD supervisord config with hhvm setup
ADD supervisord.conf /etc/supervisord.conf
ADD scripts/supervisord /etc/rc.d/init.d/supervisord

# Step14 set to start automatically - supervisord, nginx
RUN mkdir -p /var/log/supervisor && chmod +x /etc/rc.d/init.d/supervisord && chkconfig --add supervisord && chkconfig supervisord on && chkconfig nginx on

ADD scripts/run.sh /run.sh

RUN chmod a+x /run.sh 

EXPOSE 80
#Start supervisord (which will start hhvm), nginx
ENTRYPOINT ["/run.sh"]
