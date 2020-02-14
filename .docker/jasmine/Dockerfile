FROM ubuntu:20.04

MAINTAINER AQNOUCH Mohammed <aqnouch.mohammed@gmail.com>

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN groupadd -r jasmin && useradd -r -g jasmin jasmin

ENV JASMIN_VERSION 0.9.31

# Install requirements
RUN apt-get update
RUN apt-get install -y python2.7
RUN apt-get install -y python-pip
RUN apt-get install -y python2.7-dev
RUN apt-get install -y libffi-dev
RUN apt-get install -y libssl-dev
RUN apt-get install -y rabbitmq-server
RUN apt-get install -y redis-server
RUN apt-get install -y supervisor
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*

# Install Jasmin SMS gateway
RUN mkdir -p /etc/jasmin/resource
RUN mkdir -p /etc/jasmin/store
RUN mkdir -p /var/log/jasmin
RUN chown jasmin:jasmin /etc/jasmin/store
RUN chown jasmin:jasmin /var/log/jasmin
RUN pip install --pre jasmin=="$JASMIN_VERSION"

# Change binding host for jcli
RUN sed -i '/\[jcli\]/a bind=0.0.0.0' /etc/jasmin/jasmin.cfg

EXPOSE 2775 8990 1401

COPY .docker/jasmine/docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["jasmind.py", "--enable-interceptor-client", "--enable-dlr-thrower", "--enable-dlr-lookup", "-u", "jcliadmin", "-p", "jclipwd"]
# Notes:
# - jasmind is started with native dlr-thrower and dlr-lookup threads instead of standalone processes
# - restapi (0.9rc16+) is not started in this docker configuration
