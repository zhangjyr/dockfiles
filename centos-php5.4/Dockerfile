# DOCKER-VERSION 1.0.0

FROM    centos:6.6

MAINTAINER Jingyuan Zhang, zhang@jingyuan.name

RUN yum -y install epel-release && yum -y update

ENV PHP_INI_DIR /usr/local/etc/php
RUN mkdir -p $PHP_INI_DIR/conf.d

ENV GPG_KEYS F38252826ACD957EF380D39F2F7956BC5DA04B5D
RUN set -xe \
	&& for key in $GPG_KEYS; do \
		gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	done

ENV PHP_VERSION 5.4.41
RUN buildDeps=" \
		tar \
		$PHP_EXTRA_BUILD_DEPS \
		curl-devel \
		freetype-devel \
		libjpeg-devel \
		libpng-devel \
		libmcrypt-devel \
		libxml2-devel \	
		openssl-devel \
	" \
	&& set -x \
	&& yum -y install mysql-libs gcc \
	&& yum -y install $buildDeps \
	&& curl -SL "http://php.net/get/php-$PHP_VERSION.tar.gz/from/this/mirror" -o php.tar.gz \
	&& curl -SL "http://php.net/get/php-$PHP_VERSION.tar.gz.asc/from/this/mirror" -o php.tar.gz.asc \
	&& gpg --verify php.tar.gz.asc \
	&& mkdir -p /usr/src/php \
	&& tar -xof php.tar.gz -C /usr/src/php --strip-components=1 \
	&& rm php.tar.gz* \
	&& cd /usr/src/php \
	&& ./configure \
		--with-config-file-path="$PHP_INI_DIR" \
		--with-config-file-scan-dir="$PHP_INI_DIR/conf.d" \
		$PHP_EXTRA_CONFIGURE_ARGS \
		--with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd \
		--enable-fpm \
		--disable-fileinfo \
		--enable-bcmath \
		--enable-mbstring \
		--enable-sockets \
		--enable-zip \
		--with-gd \
		--enable-gd-native-ttf \
		--with-curl \
		--with-mcrypt \
		--with-freetype-dir \
		--with-jpeg_dir \
		--with-png-dir \
		--with-openssl \
		--disable-debug \
		--disable-maintainer-zts \
		--enable-inline-optimization \
	&& make -j"$(nproc)" \
	&& make install \
	&& { find /usr/local/bin /usr/local/sbin -type f -executable -exec strip --strip-all '{}' + || true; } \
	&& yum -y erase $buildDeps \
	&& make clean 

COPY docker-php-ext-* /usr/local/bin/

COPY php-fpm.conf /usr/local/etc/

RUN groupadd -g 580 www && useradd -g www -u 580 www

EXPOSE 9000
CMD ["php-fpm"]
