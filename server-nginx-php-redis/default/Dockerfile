FROM nginx:latest

ENV LOGDIR /data/logs
ENV CACHEDIR /tmp/www

RUN groupadd -g 580 www \
    && useradd -g www -u 580 www \
    && mkdir -p $LOGDIR/nginx && chown www:www $LOGDIR/nginx \
    && mkdir -p $CACHEDIR/wanke/caches && chown www:www $CACHEDIR/wanke/caches

COPY nginx.conf /etc/nginx/nginx.conf
COPY nginx_wanke.conf /etc/nginx/conf.d/default.conf
