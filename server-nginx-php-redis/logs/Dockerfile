FROM centos:6.6

# add our user and group first to make sure their IDs get assigned consistently, regardless of other deps added later
RUN groupadd -g 580 www && useradd -g www -u 580 www

RUN mkdir -p /data/logs/nginx \
  && mkdir -p /data/logs/dev/wanke \
  && chown -R www:www /data/logs

VOLUME /data/logs
USER www
CMD ["/bin/true"]
