# DOCKER-VERSION 1.0.0

FROM    centos:6.6

MAINTAINER Jingyuan Zhang, zhang@jingyuan.name

# Step1 Install extra yum repo.
# yum update -y >/dev/null && 
RUN yum install -y http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm

# Step2 Install supervisor.
RUN yum install -y python-setuptools && easy_install supervisor

# Step3 ADD supervisord config with empty setup
ADD supervisord.conf /etc/supervisord.conf
ADD scripts/supervisord /etc/rc.d/init.d/supervisord

# Step4 set to start automatically - supervisord
RUN mkdir -p /var/log/supervisor && chmod +x /etc/rc.d/init.d/supervisord && chkconfig --add supervisord && chkconfig supervisord on

ADD scripts/run.sh /run.sh

RUN chmod a+x /run.sh 

#Start supervisord (which will start hhvm), nginx
ENTRYPOINT ["/run.sh"]
