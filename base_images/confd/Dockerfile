FROM centos:7

MAINTAINER wmiller@constantcontact.com

ADD confd/confd-0.7.1-linux-amd64 /usr/local/bin/confd

RUN mkdir -p /etc/confd/templates /etc/confd/conf.d

ADD confd/configurations/* /etc/confd/conf.d/
ADD confd/templates/* /etc/confd/templates/

RUN mkdir -p /opt/cc/etc

RUN touch /run.sh
RUN chmod +x /run.sh

RUN echo "#!/bin/sh" >> /run.sh
RUN echo "set -e" >> /run.sh
RUN echo "confd -backend="env" -onetime" >> /run.sh

CMD /run.sh
