# https://github.com/monitoringartist/zabbix-docker-monitoring

# Define required Debian version by using FROM tag. Avalaible: wheezy/jessie/...
FROM opensuse:42.2

MAINTAINER "Jan Garaj" <info@monitoringartist.com>

# Define required Zabbix version (tag/<version>) or branch (branches/<version>), e.g. tags/2.4.5, or branches/2.4  
ENV ZABBIX_VERSION=branches/3.2

WORKDIR /root

RUN \
   zypper install --no-confirm --no-recommends git subversion automake autoconf gcc make pkg-config pcre-devel 1>/dev/null && \
   git clone -q https://github.com/monitoringartist/zabbix-docker-monitoring

RUN \
   svn --quiet export svn://svn.zabbix.com/${ZABBIX_VERSION} ~/zabbix/ && \
   cd ~/zabbix/ && \
   ./bootstrap.sh 1>/dev/null && \
   ./configure --enable-agent 1>/dev/null && \
   cp -R ~/zabbix-docker-monitoring/src/modules/zabbix_module_docker/ ~/zabbix/src/modules/ && \
   cd ~/zabbix/src/modules/zabbix_module_docker && \
   make

## Dockerized compilation (build from remote URL or local PATH):
# docker build --rm=true -t local/zabbix-docker-module-compilation https://github.com/monitoringartist/zabbix-docker-monitoring.git#master:dockerfiles/debian/
# docker build --rm=true -t local/zabbix-docker-module-compilation .
# docker run  --rm -v /tmp:/tmp local/zabbix-docker-module-compilation cp /root/zabbix/src/modules/zabbix_module_docker/zabbix_module_docker.so /tmp/zabbix_module_docker.so
# docker rmi -f local/zabbix-docker-module-compilation
## use/copy /tmp/zabbix_module_docker.so in your Zabbix
