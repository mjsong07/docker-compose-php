FROM mysql:5.7-debian

LABEL maintainer="dev@my.com"

RUN apt-get update \
    && apt-get install -y iputils-ping \
    && apt-get install -y vim \
    && apt-get install -y procps \
    && apt-get install -y cron
RUN chmod -R 644 /etc/mysql
#EXPOSE 8080

