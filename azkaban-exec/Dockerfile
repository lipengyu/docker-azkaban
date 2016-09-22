# VERSION 1.0
# DOCKER HUB: https://hub.docker.com/u/lipengyu/
# DESCRIPTION: Simple image for azkaban-executor based on official alpine
# BUILD: docker build --rm -t azkaban-executor .
# SOURCE: https://github.com/lipengyu/docker-azkaban

FROM alpine

MAINTAINER Pengyu Li <lipengyu@live.com>

ENV AZK_VERSION 3.1.0
ENV MYSQL_JDBC_VERSION 5.1.35

RUN apk add --update openjdk8-jre curl bash && rm -rf /var/cache/apk/*

RUN curl -sLk https://github.com/azkaban/azkaban/releases/download/$AZK_VERSION/azkaban-exec-server-$AZK_VERSION.tar.gz | tar xz
RUN curl -sLk http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-$MYSQL_JDBC_VERSION.tar.gz| tar xz
RUN mv mysql-connector-java-$MYSQL_JDBC_VERSION/mysql-connector-java-$MYSQL_JDBC_VERSION-bin.jar azkaban-exec-server-$AZK_VERSION/extlib/
RUN rm -rf mysql-connector-java-$MYSQL_JDBC_VERSION
# Work around to run container as a daemon
RUN sed -i "s/&//" azkaban-exec-server-$AZK_VERSION/bin/azkaban-executor-start.sh

ADD conf/azkaban.properties azkaban-exec-server-$AZK_VERSION/conf/azkaban.properties

# Azkaban executor port
EXPOSE 12321

# Define default workdir
WORKDIR /azkaban-exec-server-$AZK_VERSION

ADD conf/run.sh run.sh
RUN chmod +x run.sh

CMD ["./run.sh"]
